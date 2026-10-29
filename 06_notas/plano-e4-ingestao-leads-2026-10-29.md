# Plano — E4: ligar o lead à campanha no dashboard (rota B)

**Data:** 2026-10-29 · **Origem:** decisão do Champion (Luiz Carlos Manni) em 29/10/2026 10:23 — pergunta "qual você escolhe, A ou B?" respondida **"b"** · **Natureza:** plano de implementação, pendente de autorização registrada · **Status:** ANÁLISE — nada implementado

## 1. O problema, medido

O pedido do Champion: *"precisamos ter os dados dos leads no dashboard, ligando de onde veio, campanha etc"*.

Levantamento de hoje mostra 5 elos, 3 rompidos:

| # | Elo | Estado | Evidência |
|---|---|---|---|
| 1 | Anúncio Meta envia UTM na URL | **ROMPIDO** | `url_tags` vazio nos 2 anúncios, confirmado via API em 29/10 |
| 2 | Landing captura UTM | funciona | campos `utm_*` em `leads`, lidos da URL (`src/pages/Index.tsx`) |
| 3 | Lead grava no banco 67634 | funciona | teste real em 29/10 02:46: registrou |
| 4 | Lead chega ao funil 51414 | **ROMPIDO** | nenhuma ponte; bancos independentes (confirmado: 51414 não lê 67634) |
| 5 | Dashboard mostra origem | **NÃO EXISTE** | `demandas` tem `campanha` texto livre, sem `campanha_id`/`conjunto`/`anuncio` |

**Consequência hoje:** o Adapta sabe quanto gastou (Meta) e quem se cadastrou (landing), mas não consegue ligar um ao outro. Não existe custo por lead real.

## 2. A decisão do Champion

**Rota B escolhida:** o Adapta **lê** a base de leads por rota autenticada. A landing não passa a escrever no Adapta.

**Por que B (registrado para o consultor):**
- mesmo modelo já provado no projeto com a Meta (`campanhas_meta_sync` lê via conector e grava snapshot);
- mantém os bancos independentes: queda da landing não derruba o funil;
- a landing tem `createRule` público — aceitar escrita dela no funil ampliaria superfície de ataque;
- permite janela de conciliação (o mesmo padrão de "último dado bom" da RN-F3-027).

**Custo aceito da rota B:** o dado chega com atraso (mesma cadência do agendador). Não é tempo real. O Champion foi informado.

## 3. O que precisa ser construído — 3 blocos

### Bloco 1 — UTM no anúncio (não é código; é configuração)

O campo `url_tags` do criativo está vazio. Sem ele, nenhum dos blocos seguintes funciona: o lead chega sem origem.

Valor proposto:

```
utm_source=meta&utm_medium=cpc&utm_campaign={{campaign.name}}&utm_content={{ad.name}}
```

A Meta substitui `{{campaign.name}}` e `{{ad.name}}` no momento do clique. É o mecanismo suportado pela plataforma, não gambiarra.

**Duas formas de aplicar:**
- **(a) Manual** — Gerenciador de Anúncios, editar o anúncio, campo "Parâmetros de URL". 2 minutos, o Champion faz hoje.
- **(b) Por API** — exige escrita na Meta, hoje bloqueada (RN-F3-025) e token somente leitura.

Recomendação: **(a) agora**, porque os blocos 2 e 3 dependem disso e (a) não exige emenda.

### Bloco 2 — Ponte de leitura (migration + hook)

**Migration 0026 — campos novos em `demandas`:**

| Campo | Tipo | Por quê |
|---|---|---|
| `lead_ref` | text | id do registro na landing; chave da conciliação |
| `utm_source` | text | origem (meta, linkedin, orgânico) |
| `utm_medium` | text | cpc, social, email |
| `utm_campaign` | text | nome da campanha, vindo do anúncio |
| `utm_content` | text | nome do anúncio |
| `campanha_ref` | text | `campaign_id` da Meta — liga com `campanhas_meta` |

**Migration 0027 — nova coleção `leads_ingestao`:** controle de idempotência. Campos: `lead_ref`, `origem_projeto`, `ingerido_em`, `resultado` (criado, atualizado, ignorado), `motivo_ignorado`. Garante que rodar duas vezes não duplica — mesmo padrão provado na F3-T12.

**Hook `leads_sync.js` — rota `POST /backend/v1/leads/sync`:** autenticada pela mesma chave interna do agendador. Fluxo:
1. lê a base de leads pela rota autenticada da landing (`GET /backend/v1/panorama/leads` com `X-Leads-Token`);
2. para cada lead não presente em `leads_ingestao`: cria demanda com `origem_dado = producao`, `tipo_origem = inbound`, `canal = meta` (ou o `utm_source`), `campanha` = `utm_campaign`, `lead_ref` = id;
3. grava em `leads_ingestao` (idempotência);
4. nunca atualiza dado já existente sem regra explícita — a F1 exige transição com evidência.

**Hook `leads_cron.js`:** agendador diário, nasce PAUSADO (mesmo padrão da 0023 → 0025).

### Bloco 3 — Painel de origem

Na tela Campanhas (ou aba nova "Leads"), mostrar por campanha:

| Coluna | Fonte |
|---|---|
| Campanha | `campanhas_meta.nome` |
| Gasto | `campanhas_meta.gasto` |
| Leads | contagem de `demandas` com `campanha_ref` |
| Custo por lead | gasto ÷ leads |
| Custo por página carregada | `campanhas_meta` + pixel |

**Regra dura:** se `campanha_ref` estiver vazio, a linha mostra **"origem desconhecida"** — nunca rateia gasto por suposição. É a mesma regra da SPEC-F3-005 (cartão sem origem mostra "desconhecido").

## 4. O que este plano NÃO faz

- **Não** cria público a partir da lista de leads → exige base legal (B3-LGPD-01).
- **Não** move cartão no Funil automaticamente — a F1 exige transição com evidência e a regra de qualificação segue pendente (B3-MET-01).
- **Não** apaga nem sobrescreve demanda existente.
- **Não** escreve na Meta.
- **Não** grava dado pessoal em `campanhas_meta` (RN-F3-028 preservada); o e-mail do lead fica só em `demandas.contato_ref`, como já é hoje.

## 5. Dependências e travas

| Dependência | Situação |
|---|---|
| Emenda à SPEC que autorize a ingestão de lead | **NÃO EXISTE.** F3-005 (Funil) não cobre ingestão; F3-006 (Campanhas) é só leitura Meta. **Precisa do Ramon.** |
| F3-T10 aceita (Funil Kanban) | bloqueada por F3-T01, que depende das 6 divergências documentais (prazo do Ramon: 31/10) |
| `url_tags` preenchido | ação do Champion, hoje |
| B3-LGPD-01 | permanece; não bloqueia ingestão com base legal declarada, mas bloqueia criar público |

## 6. Ordem recomendada

1. **Hoje — Champion:** preencher `url_tags` no anúncio (2 min). Sem isso, nada adianta.
2. **Esta semana — consultor:** emenda autorizando a ingestão de lead como extensão da F3-006/005. Perguntas objetivas na §7.
3. **Após (2):** implementar Bloco 2 (migrations + hooks + prova de idempotência).
4. **Após (2):** Bloco 3 (painel) — pode ir junto, é leitura.
5. **Quando F3-T10 destravar:** ligar o painel de origem ao Funil Kanban.

## 7. Perguntas objetivas ao consultor

1. A ingestão de lead da landing (projeto 67634) para o funil (51414) entra como **extensão da SPEC-F3-006** ou exige **task nova** (F3-T14)?
2. Autoriza os campos `lead_ref`, `utm_*`, `campanha_ref` em `demandas`, ou prefere coleção separada `leads` no 51414, ligada por `campanha_ref`?
3. A regra de idempotência pode ser a de `leads_ingestao` (mesmo padrão da F3-T12), ou o consultor exige identidade composta em `demandas`?
4. Ingestão diária é aceitável, ou a SPEC exige tempo real?
5. `origem_dado = producao` está correto para lead real vindo da landing, ou o consultor quer marcação distinta?

## 8. Riscos

- **LGPD:** passa a haver dado pessoal real no funil (hoje são 10 registros sintéticos). Exige a política LGPD antes de produção — é trava declarada no STATUS desde 22/10.
- **Duplicata na origem:** a landing tem índice UNIQUE em e-mail, mas o mesmo decisor pode se cadastrar com dois e-mails. A conciliação por `lead_ref` não resolve isso; é decisão de negócio.
- **Origem desconhecida vai dominar no início:** os leads anteriores a hoje não têm UTM. O painel vai mostrar "desconhecido" para todos até o `url_tags` entrar em produção. Isso é correto, não é bug.

---

*Plano produzido em 29/10/2026. Nenhuma alteração feita em banco, coleção, hook ou na Meta.*
