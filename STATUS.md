# STATUS — Projeto TG Mais Serviços de Tecnologia e RH LTDA

> **Atualizado em:** 2026-10-29 · **Por:** Consultor Adapta (Ramon Pádua)  
> Painel do projeto: fase atual, progresso, travas e próxima ação.

## Onde estamos

- **Fase atual:** 3 — Funil, painéis e passagem entre áreas (5 fases no total).
- **Fase 1:** 6/6 ✅ · **Fase 2:** 9/9 ✅ (arquivada em `05_entregas/fase-2/`).
- **Fase 3:** 1/13 tasks concluídas — F3-T09 concluída 24/10 com ressalvas preservadas (Hub 57113 ativo até substituta aprovada; leads 67634 ainda sem ponte para o funil). F3-T12 implementada, prova humana 26/26, infraestrutura de aviso/agendador comprovada em 28/10; CA-3-16 segue aberto por falta de leitura real e comparação com o Gerenciador. F3-T01 em andamento, E2 origem_dado aceita em 23/10; E2-coleções/E5/E6 sem bloqueio documental D4/D5/D6, mas ainda exigem execução autorizada, provas e teste humano. E-F3-001-04 consolidada nos documentos operacionais em 29/10, sem alteração no app.
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

F3-T01 (terminar) → F3-T09 diagnóstico → F3-T10/T11 Funil → F3-T12/T13 Campanhas → F3-T02 → demais.

## Travas que dependem de decisão

- **Política LGPD e meta (direção):** sem isso, Funil e painéis mostram só dados de teste e nenhum KPI contra meta.
- **Conexão Meta:** B3-META-LEITOR resolvido pelo aceite escrito de 28/10. Falta reconectar via OAuth e executar leitura real/comparação para CA-3-16; não pedir credenciais no chat.
- **D4/D5/D6:** decisões aprovadas em 26/10 e consolidadas em SPEC, tasks, matriz e fixtures em 29/10. Não requerem nova decisão do consultor/Champion. Retirar bloqueio documental não conclui implementação, testes nem F3-T01.
- **Matriz de responsáveis (direção):** necessária para as provas de passagem entre áreas.

## Próxima ação

1. Cliente: atualizar o contexto do Maestro com os arquivos consolidados; não reabrir D1–D6 nem refazer etapas já aceitas.
2. F3-T12 segue ativa: reconectar a Meta e comprovar leitura/comparação CA-3-16. Se for priorizar F3-T01 enquanto o acesso não volta, o Champion deve registrar pausa segura da T12 (cron pausado, evidências preservadas). Não executar duas tasks simultâneas.
3. Retomada da F3-T01: conferir implementação real, explicar o próximo passo, obter autorização do Champion e executar somente o trecho pendente. E2 origem_dado aceita não deve ser refeita; E2-coleções/E5/E6 não estão aceitas por esta correção. Rodar provas e aguardar teste humano antes de concluir.
4. F3-T10 só fica elegível após aceite integral da F3-T01.
5. E4 (ingestão de leads): plano de 29/10 sem emenda/task liberada. Não implementar ingestão nem usar dado pessoal real por esta correção; política LGPD e demais autorizações continuam obrigatórias. Multi-tenant e autonomia de tráfego não fazem parte desta consolidação.

**Prazo de referência da cobrança documental:** 31/10/2026; consolidação publicada em 29/10. Não é promessa de conclusão de F3-T01, Funil ou E4 nessa data.
