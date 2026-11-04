# Fase 3 — Tarefas

<!-- fase-format:2 -->

**Plano:** TG Mais Serviços de Tecnologia e RH LTDA · `6b09d189`  
**Estado:** Fase 3 em execução; 3/13 tasks concluídas — F3-T09 (com ressalvas) e **F3-T12 CONCLUÍDA em 03/11/2026** (tela Campanhas lendo a Meta por Graph API direta; leitura real comprovada; CA-3-16 atestado pelo owner; conferência final da tela contra a API campo a campo). **F3-T01 CONCLUÍDA em 04/11/2026**: propostas/vagas, histórico append-only, retorno da operação e recuperação 3/90 implementados e testados pelo owner. E2 origem_dado aceita em 23/10 e preservada.
**Champion:** João Paulo  
**Atualizado em:** 04/11/2026 pelo Maestro — F3-T01 concluída após teste humano do owner; quatro defeitos reais encontrados e corrigidos durante a implementação (registro em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`).  
**Regra:** uma task por vez; cada conclusão exige as provas da SPEC e o teste humano do Champion.

## Esta fase em linguagem simples

Na Fase 3 o sistema passa a **mostrar** o caminho de cada demanda. Ao final, vocês vão ter no Skip (projeto **"repositório adapta"**):

- **Funil** — um quadro em colunas (Kanban): cada cartão é uma empresa/demanda e mostra de onde veio (inbound ou outbound, canal, campanha), o que ela quer (R&S ou TMO), quem é o dono e a próxima ação. Clicando, aparece a história completa.
- **Campanhas** — as campanhas Meta Ads (ativas ou pausadas, gasto, resultados), atualizadas todo dia por uma automação no Maestro, com aviso se a conexão cair.
- **Gargalos** — quantas demandas há em cada etapa e onde estão travando.
- **Passagens pendentes** — quando o comercial passa uma demanda para o R&S ou a alocação, para quem foi e se foi aceita.
- **Área do DHO** — separada, só para o DHO.

## Ordem de execução

F3-T01 → F3-T09 → F3-T10 → F3-T11 → F3-T12 → F3-T13 → F3-T02 → F3-T03 → F3-T04 → F3-T05 → F3-T06 → F3-T07 → F3-T08

## Situação vigente e retomada

**Consolidação documental de 29/10/2026:** E-F3-001-04 vigente; D1–D6 decididas. E2-coleções/E5/E6 não aguardam nova decisão D4/D5/D6. A implementação pendente continua sujeita à autorização do Champion, às provas da SPEC e ao teste humano.

**Atualização de 03/11/2026 — F3-T12 CONCLUÍDA:** o owner conferiu a tela `/campanhas` recarregada e os sete números do último retrato batem com a Graph API (10 campanhas · 1 ativa · gasto R$ 17,56 · 4.663 impressões · 184 cliques · 113 resultados). Três defeitos reais foram encontrados pela conferência visual e corrigidos: `normalizarStatus` convertendo o booleano da comparação (v0.0.118), ausência de porta de entrada própria na tela (v0.0.119) e o campo `resultados` somando os 13 tipos de `actions` da Meta (v0.0.120). Registro: `05_entregas/campanhas-f3.md`; aceite: `06_notas/aceite-graph-api-f3-t12-2026-10-30.md`.

**Atualização de 04/11/2026 — F3-T01 CONCLUÍDA:** implementadas as coleções `propostas` e `vagas` ligadas à demanda (migração 0026), o histórico append-only `pipeline_log` (0027), os campos de recuperação comercial em `demandas` (0028) e os três cenários sintéticos PIPE-F3-IN-001/OUT-001/STUCK-001 (0029). Regras: filho sem demanda válida é rejeitado; empresa não existe no filho; proposta/vaga não têm estado de pipeline; "Substituição encerrada" exige motivo "reposto" ou "encerrada sem reposição" e não encerra vaga nem demanda; recuperação limitada a 3 tentativas em 90 dias corridos contados da **primeira tentativa formal humana**, sem reinício por novas tentativas, mudanças de etapa ou automações (decisão do Champion de 04/11/2026). Tela `/pipeline-t01` mostra propostas, vagas, retorno e a jornada reconstruída do histórico. Teste humano aprovado pelo owner em 04/11 ("perfeito. vi aqui. ok tudo certo"). Registro completo em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`.

Com a T01 concluída, a próxima é a **F3-T13**, que exige autorização do Champion. A ordem abaixo não reabre F3-T09 nem autoriza duas tasks simultâneas. F3-T10 segue dependente de F3-T01 integralmente aceita — requisito atingido em 04/11.

## Jornada operacional

- [x] Materializar campos de proposta/vaga, retorno e escalonamento no pipeline com massa sintética @"João Paulo" #projeto
  > F3-T01 · SPEC F3-001 · CA-3-01/02/04 (E-F3-001-01). Criar fixtures inbound/outbound; percorrer o pipeline até proposta/vaga com retorno; exercitar escalonamento de oportunidade parada; demonstrar hierarquia Demanda→Propostas→Vagas; registrar captura/export e log de transições. Parar se exigir dado real, dedupe por inferência ou integração externa.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: cada demanda de teste ganha propostas, vagas, retorno da operação e aviso quando fica parada.
  > **Concluída em 04/11/2026** — teste humano do owner aprovado; evidência em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`.
- [ ] Provar jornada dupla, retorno da operação, escalonamento e equivalência entre duas pessoas @"João Paulo" #projeto
  > F3-T02 · SPEC F3-001 · CA-3-01..04. Executar RED/GREEN/regressão, replay, contagens pela fonte, hierarquia Demanda→Propostas→Vagas e bateria de equivalência. Depende de F3-T01 aceita e da matriz RACI nominal para a prova de handoff.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: um roteiro em que duas pessoas classificam os mesmos casos e chegam à mesma etapa, com "passou / não passou" por caso.
- [ ] Materializar visões de gargalo, tempos e retorno no dashboard com massa sintética @"João Paulo" #projeto
  > F3-T03 · SPEC F3-002 · CA-3-11 (E-F3-002-01). Criar visões por estado, gargalo, tempos e retorno; manter o KPI contra alvo bloqueado e a pendência B3-MET-01 visível. Depende de F3-T01 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: tela "Gargalos" com quantas demandas há em cada etapa, tempo entre etapas e quais estão paradas além do prazo.
- [ ] Provar leitura de gargalo, bloqueio de KPI sem alvo e reprodução das contagens @"João Paulo" #projeto
  > F3-T04 · SPEC F3-002 · CA-3-11/12 (E-F3-002-01). Provar leitura completa, contagens reproduzíveis, separação volume×qualidade e bloqueio sem G-001; regressão do painel F1/F2. Depende de F3-T03 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a direção abre a tela e diz onde a demanda trava; números iguais ao Funil; meta como "pendente de decisão".
- [ ] Materializar registro de handoff com SLA, escalonamento e recuperação com massa sintética @"João Paulo" #projeto
  > F3-T05 · SPEC F3-003 · CA-3-06. Criar HO-F3-001 com destinatário, SLA e estado; exercitar aceite, recusa e expiração com papéis provisórios registrados. Depende de F3-T01 aceita.
