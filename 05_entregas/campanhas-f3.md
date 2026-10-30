# Evidência — Tela Campanhas (F3-T12 · SPEC-F3-006)

**Data:** 2026-10-28 · **Última atualização:** 2026-10-30 · **Versão Skip:** 0.0.118 (ref `edcd38e`) · **Champion:** João Paulo · **Owner:** Luiz Carlos Manni

## 1. O que foi entregue

| Peça | Onde | Estado |
|---|---|---|
| Coleção `campanhas_meta` (retrato diário por campanha) | Skip 51414 | Criada — índice ÚNICO `campaign_id` + `data_referencia`; escrita bloqueada na API (só o hook grava) |
| Coleção `conexao_meta` (saúde da conexão) | Skip 51414 | Criada — 1 registro por execução |
| Migration `0022_campanhas_meta` | código | Aplicada (não destrutiva; rollback remove só as duas coleções) |
| Migration `0023_campanhas_meta_cron_pausado` | código | Aplicada — **agendador nasce pausado** |
| Migration `0024_conta_servico_agendador` | código | Aplicada — conta de serviço com role vazio |
| Migration `0025_campanhas_meta_cron_ativo` | código | Aplicada — agendador a cada 2h |
| Núcleo puro (regras da SPEC) | `src/lib/f3/t12/nucleo.ts` | Implementado |
| Bateria de prova | `src/lib/f3/t12/prova.ts` | 26 checagens |
| Serviço de leitura | `src/services/campanhasMeta.ts` | Implementado — nenhuma função escreve |
| Tela Campanhas | `/campanhas` | Publicada no preview |
| Tela de prova | `/campanhas/prova` | Publicada no preview |
| Automação (rota manual + porta interna + cron) | `pocketbase/hooks/campanhas_meta_{sync,run,cron}.js` | Implementada; cron a cada 2h |

## 2. QA do Skip (v0.0.118)

setup OK · análise estática OK · build OK · integrações OK · suíte do projeto não roda (`ran: false`).

## 3. Prova RED/GREEN/REGRESSÃO — 26/26 (evidência do Champion, 28/10 11:22)

Captura enviada pelo Champion (`maestro1.pdf`, 3 páginas, preview `/campanhas/prova`):

- **RED — falhas obrigatórias: 11/11** (credencial expirada → `expirada` + e-mail; 403 → `expirada`; 429 → `erro`; timeout sem retry em sequência; falha não gera snapshot; dado pessoal e campo fora do contrato descartados; destino em outro projeto recusado; coleção fora do contrato recusada; ficha completa; automação sem task recusada; campanha sem identificador ignorada).
- **GREEN — caminho principal: 10/10** (uma linha por campanha; campos do contrato; objetivo sem prefixo técnico; data de referência válida; primeira execução cria; reexecução não duplica; lote repetido grava uma vez; faixa de saúde por estado; nunca "ok" sem registro; falha mostra o último dado bom).
- **REGRESSÃO: 5/5** (status bruto → vocabulário fechado; chave de idempotência; data inválida rejeitada; formatação BR; zero é zero).

**Correção real encontrada pela própria prova:** a primeira execução deu **25/26** — o núcleo não lia `actions` (formato em que a Meta devolve "resultados") e contava aliases legítimos (`id`, `name`, `effective_status`) como campos descartados. Corrigido e comprovado em 26/26.

## 4. Leitura real da Meta — EXECUTADA em 30/10/2026

A pendência declarada em 28/10 (§4 da versão anterior) está **fechada**. Duas mudanças tornaram isso possível:

1. **Fonte trocada de Composio para Graph API v21.0 direta** (v0.0.117). A conexão `adapta-tg` do Composio nunca completou o consentimento e ficou em `INITIATED`; a leitura real, por esse caminho, nunca saía.
2. **Token do principal guardado no segredo `META_ACCESS_TOKEN`** do projeto 51414, lido em runtime por `$secrets.get`. Principal: **João Paulo Oliveira** (id `28957746507189663`) — o mesmo nomeado no aceite B3-META-LEITOR de 28/10. Escopo `ads_read`; sem expiração.

### Execução comprovada — 30/10/2026 02:47 UTC

Retorno do próprio hook `POST /backend/v1/campanhas-meta/run`:

| Item | Resultado |
|---|---|
| `status` | `ok` |
| `campanhas_lidas` | **10** |
| `linhas_gravadas` | **70** (10 campanhas × 7 dias) |
| `criadas` / `atualizadas` | 0 / 70 |
| `erros` | `[]` |
| `somente_leitura_meta` | `true` |
| Janela | 22/10 a 28/10/2026 |

### Correção de um diagnóstico anterior — registrar para não repetir

Em 29/10 foi registrado que o hook chamava a listagem **sem** `effective_status` e por isso via 1 campanha, escondendo R$ 146,19. **Esse diagnóstico estava errado.** O log `scripts/f3t12-rede-seguranca/execucoes.log` de **29/10 15:05 UTC, antes de qualquer alteração**, já registrava `linhas=70` = 10 campanhas × 7 dias: o Composio devolvia as 10. O que se mediu foi a **Graph API crua**, não o hook.

O parâmetro `effective_status` foi mantido e é **necessário a partir de 30/10**, porque a fonte passou a ser a Graph API direta — sem ele, a tela mostraria 1 campanha. Ele previne um defeito que a troca de fonte teria criado, e não corrige um defeito anterior.

## 5. CA-3-16 — atendido e atestado por humano

**Atestação do owner (Luiz Carlos Manni), 30/10/2026:** conferência no Gerenciador de Anúncios, janela 22–28/10/2026, contra a tela `/campanhas` — resultado declarado: **"bateu"**.

Números conferidos (fonte: Graph API; conferência humana no Gerenciador):

| Campanha | Status | Gasto 7d | Impressões | Cliques |
|---|---|---:|---:|---:|
| [TG] [TESTE] [PANORAMA] [19/10/2026] | Arquivada | R$ 146,19 | 41.437 | 2.705 |
| [TG] [TESTE] [PANORAMA] [19/10/2026] — Cópia | Ativa | R$ 0,00 na janela¹ | 0¹ | 0¹ |
| Demais 8 campanhas (AYPLA, LEADS, RETARGETING) | Arquivadas | R$ 0,00 | 0 | 0 |

¹ criada em 28/10, começou a veicular em 29/10 — o gasto dela fica fora da janela 22–28/10.

**Documento de apoio:** `artifacts/CA-3-16_Folha_de_Conferencia_2026-10-30.md` (fora do repositório; gerado pelo Maestro).
**Aceite da fonte:** `06_notas/aceite-graph-api-f3-t12-2026-10-30.md`.

## 6. Nada foi escrito na Meta

A automação só faz **GET** em `/campaigns` e `/insights` da Graph API; não existe no código nenhum POST, PUT ou DELETE contra a Meta (RN-F3-025). Verificado por leitura dos três hooks e confirmado no retorno de cada execução (`somente_leitura_meta: true`).

## 7. Pendências que permanecem (não bloqueiam o CA-3-16)

| Pendência | Situação |
|---|---|
| Conferência diária do agendador | cron interno a cada 2h + rede de segurança externa no ETHOS às 10:30 BRT |
| Instância hiberna | cron interno morre com a instância, sem replay; por isso a rede de segurança externa |
| `COMPOSIO_API_KEY` | permanece no projeto, sem uso pela T12 desde 30/10 |
| CA-3-17 (F3-T13) | depende do aceite da F3-T12; provas de falha, idempotência e destino único |

---

## 8. Correção pós-atestação (30/10, v0.0.118, ref `edcd38e`)

Ao abrir a tela `/campanhas`, o owner viu todas as situações como **"Desconhecido"**. Causa raiz, no hook `campanhas_meta_sync.js`:

```js
// ERRADO — convertia o RESULTADO BOOLEANO da comparação, não o valor:
const s = String(v === undefined || v === null || v === '').trim().toLowerCase()
// String(false) = 'false' → nenhum valor válido batia → tudo virava 'desconhecido'
```

Correção: `String(v === undefined || v === null || v === '' ? '' : v)`. Nova execução real em 30/10: 10 campanhas, 70 linhas atualizadas, `status_efetivo` correto (1 `active`, 9 `archived`). QA do Skip: setup/static/build/integrations OK.

**Dois comportamentos da tela confirmados neste diagnóstico (não são defeitos):**

1. **Sem login, a tela mostra zeros e "Sem informação da conexão".** As coleções têm `listRule: @request.auth.id != ''` (leitura só para autenticados, por design da SPEC). O acesso se estabelece pelo botão **"Entrar"** na tela Experimentos (`/experimentos`); a sessão fica guardada no navegador.
2. **Consulta anônima à API devolve `totalItems: 0` mesmo com dado gravado** — não concluir "banco vazio" a partir de leitura sem sessão. Prova: sem sessão 0 registros; com sessão, 80 registros (28–29/10).

---

*Evidência atualizada em 30/10/2026. Versão no ar: Skip 51414 v0.0.118, ref `edcd38e`.*
