# STATUS — Projeto TG Mais Serviços de Tecnologia e RH LTDA

> **Atualizado em:** 2026-11-11 · **Por:** Maestro (F3-T10 concluída após homologação da etapa E5)  
> Painel do projeto: fase atual, progresso, travas e próxima ação.

## Onde estamos

- **Fase atual:** 3 — Funil, painéis e passagem entre áreas (5 fases no total).
- **Fase 1:** 6/6 ✅ · **Fase 2:** 9/9 ✅ (arquivada em `05_entregas/fase-2/`).
- **Fase 3:** 5/13 tasks concluídas — **F3-T09** (24/10, com ressalvas preservadas: Hub 57113 ativo até substituta aprovada; leads do 67634 ainda sem ponte para o funil), **F3-T12 CONCLUÍDA em 03/11/2026** (tela Campanhas lendo a Meta por Graph API direta; CA-3-16 atestado pelo owner), **F3-T01 CONCLUÍDA em 04/11/2026** (propostas e vagas ligadas à demanda, histórico append-only, retorno da operação e recuperação 3/90), **F3-T13 CONCLUÍDA em 04/11/2026** (provas de falha com credencial inválida real: e-mail confirmado no log, faixa de aviso na tela, retrato anterior intacto, zero escrita na Meta) e **F3-T10 CONCLUÍDA em 11/11/2026**: tela Funil entregue em 5 etapas, cada uma homologada pelo owner — colunas e contagens, cartão completo, filtros, Jornada e arrastar com evidência. A prova final foi dele: moveu "Serviços Delta" de Oportunidade para Proposta e o sistema registrou a transição no histórico. Quatro defeitos reais corrigidos no caminho.
- **Champion:** João Paulo.
- **Prazo:** sem trava de prazo (decisão de 07/10/2026).

## O que mudou em 22/10 (5ª call)

- Novas telas na Fase 3: **Funil** (Kanban com origem e história de cada demanda) e **Campanhas**
  (Meta Ads, por automação diária no Maestro). A Fase 3 passou de 8 para 13 tasks.
- Toda task agora diz **onde executar** e **o que você vai ver** no final.
- O Maestro passa a conduzir passo a passo: explica a fase e a task, combina cada passo, pede para
  conferir no Skip e fecha com resumo.
- Todo o projeto vive no Skip **"repositório adapta"**; a automação que mandava dados para o "Hub de
  gestão Meta Ads" deve ficar parada.
- RD Station não faz parte do projeto.

## Ordem das próximas tasks

F3-T01 ✅ → F3-T13 ✅ → **F3-T11** (próxima, as provas do Funil) → T03/T05 → demais. Concluídas: F3-T09, F3-T12, F3-T01, F3-T13 e F3-T10.

## Travas que dependem de decisão

- **Política LGPD e meta (direção):** sem isso, Funil e painéis mostram só dados de teste e nenhum KPI contra meta.
- **Conexão Meta:** B3-META-LEITOR resolvido pelo aceite de 28/10 e **reafirmado em 30/10** para a fonte Graph API direta (`06_notas/aceite-graph-api-f3-t12-2026-10-30.md`); token no segredo `META_ACCESS_TOKEN`. **O token tem poder de escrita** (já criou e excluiu campanhas); a automação só faz GET. Não pedir credenciais no chat.
- **D4/D5/D6:** decisões aprovadas em 26/10 e consolidadas em 29/10. Implementadas e provadas na F3-T01 (04/11).
- **Matriz de responsáveis (direção):** necessária para as provas de passagem entre áreas (F3-T05/T06) e para a prova de equivalência da F3-T02 (B3-RACI-01). **É o próximo insumo a nomear.**

## Próxima ação

1. Cliente: F3-T10 concluída em 11/11. Autorizar a **F3-T11** (provas do Funil) ou outra task liberada; a T11 é a próxima na ordem.
2. F3-T12 CONCLUÍDA em 03/11. A automação diária segue rodando (cron a cada 2h) para manter o retrato atualizado.
3. F3-T01 CONCLUÍDA em 04/11: migrações 0026-0029, ganchos de hierarquia/recuperação/append-only e tela `/pipeline-t01`. Evidência em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`.
4. F3-T13 CONCLUÍDA em 04/11: v0.0.146, provas de falha real, idempotência, destino único e zero escrita na Meta. Evidência em `06_notas/changelog-2026-11-04-f3-t13-concluida.md`.
5. F3-T10 CONCLUÍDA em 11/11: tela `/funil` com 5 etapas homologadas, versão v0.0.158. Evidência em `06_notas/changelog-2026-11-11-f3-t10-concluida.md`.
6. E4 (ingestão de leads): plano de 29/10 sem emenda/task liberada. Não implementar ingestão nem usar dado pessoal real; política LGPD continua obrigatória.

**Prazo de referência da cobrança documental:** 31/10/2026; consolidação publicada em 29/10. Não é promessa de conclusão de Funil ou E4 nessa data.
