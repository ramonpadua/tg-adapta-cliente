# STATUS — Projeto TG Mais Serviços de Tecnologia e RH LTDA

> **Atualizado em:** 2026-11-04 · **Por:** Maestro (F3-T01 concluída após teste humano do owner)  
> Painel do projeto: fase atual, progresso, travas e próxima ação.

## Onde estamos

- **Fase atual:** 3 — Funil, painéis e passagem entre áreas (5 fases no total).
- **Fase 1:** 6/6 ✅ · **Fase 2:** 9/9 ✅ (arquivada em `05_entregas/fase-2/`).
- **Fase 3:** 3/13 tasks concluídas — **F3-T09** (24/10, com ressalvas preservadas: Hub 57113 ativo até substituta aprovada; leads do 67634 ainda sem ponte para o funil), **F3-T12 CONCLUÍDA em 03/11/2026** (tela Campanhas lendo a Meta por Graph API direta; leitura real comprovada: 10 campanhas, 70 linhas; CA-3-16 atestado pelo owner; conferência campo a campo contra a API) e **F3-T01 CONCLUÍDA em 04/11/2026**: propostas e vagas ligadas à demanda, histórico de transições append-only (não se edita nem se apaga), retorno da operação com "Substituição encerrada" exigindo motivo, e recuperação comercial 3 tentativas/90 dias contados da primeira tentativa humana. Testada pelo owner. **F3-T13 é a próxima.**
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

F3-T01 ✅ → **F3-T13** (próxima) → F3-T10/T11 Funil → F3-T02 → demais. F3-T09 e F3-T12 concluídas.

## Travas que dependem de decisão

- **Política LGPD e meta (direção):** sem isso, Funil e painéis mostram só dados de teste e nenhum KPI contra meta.
- **Conexão Meta:** B3-META-LEITOR resolvido pelo aceite de 28/10 e **reafirmado em 30/10** para a fonte Graph API direta (`06_notas/aceite-graph-api-f3-t12-2026-10-30.md`); token no segredo `META_ACCESS_TOKEN`, somente leitura. Leitura real e comparação do CA-3-16 concluídas. Não pedir credenciais no chat.
- **D4/D5/D6:** decisões aprovadas em 26/10 e consolidadas em SPEC, tasks, matriz e fixtures em 29/10. Implementadas e provadas na F3-T01 (04/11).
- **Matriz de responsáveis (direção):** necessária para as provas de passagem entre áreas (F3-T05/T06) e para a prova de equivalência da F3-T02 (B3-RACI-01). **É o próximo insumo a nomear.**

## Próxima ação

1. Cliente: apresentar a **F3-T13** e obter autorização; nomear a matriz RACI nominal (B3-RACI-01).
2. F3-T12 CONCLUÍDA em 03/11 (owner conferiu a tela e os números batem). A automação diária segue rodando para manter o retrato atualizado.
3. F3-T01 CONCLUÍDA em 04/11: migrações 0026-0029, ganchos de hierarquia/recuperação/append-only e tela `/pipeline-t01`. Evidência registrada em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`.
4. F3-T10 só fica elegível após aceite integral da F3-T01 — **atingido em 04/11**.
5. E4 (ingestão de leads): plano de 29/10 sem emenda/task liberada. Não implementar ingestão nem usar dado pessoal real; política LGPD e demais autorizações continuam obrigatórias. Multi-tenant e autonomia de tráfego não fazem parte desta consolidação.

**Prazo de referência da cobrança documental:** 31/10/2026; consolidação publicada em 29/10. Não é promessa de conclusão de Funil ou E4 nessa data.
