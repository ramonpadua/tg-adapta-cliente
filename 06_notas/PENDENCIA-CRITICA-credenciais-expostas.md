# PENDÊNCIA CRÍTICA BLOQUEANTE — credenciais expostas no código público

> **Status: RESOLVIDA EM 12/11/2026 (v0.0.179) — zero ocorrências no arquivo público**
> Registrada em 11/11/2026 19:06 por decisão do owner.
> **Falta apenas:** revogar/desativar a conta de teste, o que depende da confirmação de acesso dos dois administradores.

## Regra de bloqueio

**O sistema NÃO pode ser liberado para uso real enquanto a pendência estiver aberta.**

Liberar para uso real inclui, sem exceção:
- publicar em produção;
- conectar dados reais de clientes, candidatos ou campanhas;
- criar contas para pessoas da empresa que não sejam de teste;
- sair do ambiente de homologação.

**Até a resolução: somente dados fictícios.**

## O que estava exposto

A senha do usuário de teste `humano-sintetico-aprovador-01@f2.invalid` — senha `F2-Sintetico-2026!` — estava escrita em texto aberto no código do aplicativo, que é **público por definição**: qualquer navegador que abra o sistema baixa o arquivo inteiro.

**Ocorrências verificadas:** 6 cópias da mesma credencial, em 6 telas.

| # | Arquivo | Tela |
|---|---|---|
| 1 | `src/pages/CampanhasF3Page.tsx` | Campanhas (desde 28/10) |
| 2 | `src/pages/PipelineF3T01Page.tsx` | Pipeline (T01) |
| 3 | `src/pages/ProvaF3T13Page.tsx` | Prova da conexão Meta |
| 4 | `src/pages/ExperimentosF2.tsx` | Experimentos (Fase 2) |
| 5 | `src/components/seg/AcessoRequerido.tsx` | Visão geral (Inicial) |
| 6 | `src/components/seg/ProtecaoDeRota.tsx` | Funil |

**Poder da credencial:** papel `champion` — o mais alto disponível. Permite ler tudo, aprovar e bloquear na Fase 2, criar e alterar decisões.

**Limites que as Etapas 1 e 2 já impõem:** não cria conta, não muda papel, não apaga demanda.

**Laudo completo:** `artifacts/LAUDO-credenciais-expostas-2026-11-11.md`

## O que tem de ser feito antes de liberar

Três etapas obrigatórias, nesta ordem:

1. **Remover todas as senhas do código** — as seis cópias. Cada pessoa digita a própria credencial. ✅ FEITO em 12/11
2. **Revogar as credenciais antigas** — trocar a senha do usuário de teste e desativá-lo, ou apagá-lo. Uma senha que já foi pública não volta a ser segura. ⏳ PENDENTE (depende de manni@ confirmar acesso)
3. **Validar a segurança dos acessos** — conferir de novo o arquivo público procurando segredos; testar que ninguém entra sem credencial válida; confirmar que as proteções das Etapas 1, 2, 5 e 6 continuam ativas. ✅ FEITO em 12/11 (zero ocorrências)

## Onde isso entra no plano

- **Etapa 4** (contas reais) é o gatilho: ao criar as contas verdadeiras, as credenciais saem do código e cada pessoa usa a sua.
- **Etapa 5** (fechar leitura pública de demandas) só faz sentido **depois** dos itens 1 e 2 acima.

## Verificação de que a pendência continua aberta

```bash
curl -s "https://repositorio-adapta-cc556--preview.goskip.app/funil" \
  | grep -o "index-[A-Za-z0-9_-]*\.js" | head -1
# depois:
curl -s "https://repositorio-adapta-cc556--preview.goskip.app/assets/<arquivo>.js" \
  | grep -c "F2-Sintetico-2026!"
```

**Resultado em 12/11/2026, v0.0.179: `0`**

---

## Histórico

- **11/11/2026 19:05** — owner pediu verificação de credenciais expostas antes de liberar a próxima rota.
- **11/11/2026 19:05** — verificação feita: 6 cópias encontradas. Nenhum outro segredo exposto.
- **11/11/2026 19:06** — owner decidiu: seguir com a Etapa 3 e tratar na Etapa 4; registrada como **pendência crítica obrigatória**.
- **12/11/2026** — **6 cópias removidas**; todas as telas passam a pedir e-mail e senha. Verificação no arquivo público: **0 ocorrências** da senha, do e-mail de teste e de qualquer token.

---

# RESOLUÇÃO — 12/11/2026 (v0.0.179)

## O que foi feito

As 6 cópias da credencial foram removidas do código. Cada tela agora **pede e-mail e senha** de quem usa, em vez de entrar sozinha.

| # | Arquivo | Situação |
|---|---|---|
| 1 | `src/pages/CampanhasF3Page.tsx` | credencial removida → `FormularioDeAcesso` |
| 2 | `src/pages/PipelineF3T01Page.tsx` | idem |
| 3 | `src/pages/ProvaF3T13Page.tsx` | idem |
| 4 | `src/pages/ExperimentosF2.tsx` | idem (cartão próprio, agora com campos) |
| 5 | `src/components/seg/AcessoRequerido.tsx` | idem |
| 6 | `src/components/seg/ProtecaoDeRota.tsx` | idem |

Novo componente: `src/components/seg/FormularioDeAcesso.tsx` — formulário único reutilizado pelas telas.

## Verificação objetiva

Arquivo que o navegador recebe, versão **v0.0.179** (`index-4Q6a9_nX.js`, 2.042.101 bytes):

| Procura | Resultado |
|---|---|
| `F2-Sintetico-2026!` | **0 ocorrências** |
| `humano-sintetico-aprovador-01` | **0 ocorrências** |
| Token da Meta (`EAA…`) | **0 ocorrências** |

## Testes

| Teste | Resultado |
|---|---|
| Sem sessão, abrir Funil | formulário com e-mail e senha, sem cartões |
| Preencher e entrar | autentica e abre com 13 de 13 cartões |
| Campanhas | 10 campanhas, conexão Meta ativa |
| Pipeline (T01) | PIPE-F3-IN-001 visível |
| Prova Meta | conteúdo completo |
| Experimentos F2 | conteúdo completo |
| Proteções de banco | escrita anônima 404; criar conta 403 |
| Dados | demandas 13, propostas 2, vagas 2, histórico 9 — intactos |

## O que ainda falta

**Revogar/desativar a conta de teste `humano-sintetico-aprovador-01`.** A senha dela saiu do código — ninguém a obtém mais pelo navegador — mas a conta continua ativa e serve de rede de segurança. Só será desativada depois que as **duas** contas reais confirmarem acesso: `jpaulo@` já confirmou; `manni@` ainda não.
