# Evidência — Tela Campanhas (F3-T12 · SPEC-F3-006)

**Data:** 2026-10-28 · **Versão Skip:** 0.0.101 (ref `06fb4ca`) · **Champion:** João Paulo

## 1. O que foi entregue

| Peça | Onde | Estado |
|---|---|---|
| Coleção `campanhas_meta` (retrato diário por campanha) | Skip 51414 | Criada — índice ÚNICO `campaign_id` + `data_referencia`; escrita bloqueada na API (só o hook grava) |
| Coleção `conexao_meta` (saúde da conexão) | Skip 51414 | Criada — 1 registro por execução |
| Migration `0022_campanhas_meta` | código | Aplicada (não destrutiva; rollback remove só as duas coleções) |
| Migration `0023_campanhas_meta_cron_pausado` | código | Aplicada — **agendador nasce pausado** |
| Núcleo puro (regras da SPEC) | `src/lib/f3/t12/nucleo.ts` | Implementado |
| Bateria de prova | `src/lib/f3/t12/prova.ts` | 26 checagens |
| Serviço de leitura | `src/services/campanhasMeta.ts` | Implementado — nenhuma função escreve |
| Tela Campanhas | `/campanhas` | Publicada no preview |
| Tela de prova | `/campanhas/prova` | Publicada no preview |
| Automação (rota manual + porta interna + cron) | `pocketbase/hooks/campanhas_meta_{sync,run,cron}.js` | Implementada; **cron pausado** |

## 2. QA do Skip (v0.0.101)

setup OK · análise estática OK · build OK · integrações OK · suíte do projeto não roda (`ran: false`).

## 3. Prova RED/GREEN/REGRESSÃO — 26/26 (evidência do Champion, 28/10 11:22)

Captura enviada pelo Champion (`maestro1.pdf`, 3 páginas, preview `/campanhas/prova`):

- **RED — falhas obrigatórias: 11/11** (credencial expirada → `expirada` + e-mail; 403 → `expirada`; 429 → `erro`; timeout sem retry em sequência; falha não gera snapshot; dado pessoal e campo fora do contrato descartados; destino em outro projeto recusado; coleção fora do contrato recusada; ficha completa; automação sem task recusada; campanha sem identificador ignorada).
- **GREEN — caminho principal: 10/10** (uma linha por campanha; campos do contrato; objetivo sem prefixo técnico; data de referência válida; primeira execução cria; reexecução não duplica; lote repetido grava uma vez; faixa de saúde por estado; nunca "ok" sem registro; falha mostra o último dado bom).
- **REGRESSÃO: 5/5** (status bruto → vocabulário fechado; chave de idempotência; data inválida rejeitada; formatação BR; zero é zero).

**Correção real encontrada pela própria prova:** a primeira execução deu **25/26** — o núcleo não lia `actions` (formato em que a Meta devolve "resultados") e contava aliases legítimos (`id`, `name`, `effective_status`) como campos descartados. Corrigido e comprovado em 26/26.

## 4. O que a prova NÃO cobre (pendência declarada)

**A leitura real da Meta não foi executada.** Portanto o **CA-3-16 não está atendido** na sua exigência central — "iguais ao Gerenciador de Anúncios na mesma data de referência". A tela abre, mostra a faixa honesta ("Sem informação da conexão") e zeros legítimos, mas ainda não há número real de campanha.

Motivos, ambos externos ao código:

1. **Acesso à Meta inválido** — o passe do conector usado pela automação está vencido desde 19/10 (erro 190/401 confirmado em 28/10). A automação usa a chave `COMPOSIO_API_KEY` (server-side) e não se sabe qual conta Meta está vinculada a ela nem se segue válida.
2. **Conta de serviço do agendador não configurada** — para o robô rodar às 06:20 sem ninguém logado, faltam `SCHEDULER_SERVICE_EMAIL`/`SCHEDULER_SERVICE_PASSWORD`; e `CHAMPION_EMAIL` para o aviso de falha sair por e-mail (sem ele, o aviso fica só na faixa da tela).

## 5. Nada foi escrito na Meta

A automação só chama `METAADS_LIST_CAMPAIGNS` e `METAADS_GET_INSIGHTS`; não existe no código nenhuma ação de escrita (RN-F3-025). Verificado por leitura dos três hooks.

## 6. Para fechar o CA-3-16

1. Restabelecer o acesso à Meta usado pela automação (renovar/conectar a conta em `act_1667348577717128`).
2. Configurar `CHAMPION_EMAIL` e a conta de serviço do agendador.
3. Executar a leitura manual e comparar, campanha por campanha, com o Gerenciador de Anúncios na mesma data de referência.
4. Só então ligar o agendador (migration com `disabled: false`).
