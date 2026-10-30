# Fase 3 — Tarefas

<!-- fase-format:2 -->

**Plano:** TG Mais Serviços de Tecnologia e RH LTDA · `6b09d189`  
**Estado:** Fase 3 em execução; 1/13 tasks concluídas (F3-T09, com ressalvas). F3-T12 com leitura real executada em 30/10 (10 campanhas, 70 linhas) e **CA-3-16 atestado pelo owner** ("bateu"); aguarda confirmação do teste humano para concluir. F3-T01 em andamento; E2 origem_dado aceita em 23/10; E2-coleções/E5/E6 documentalmente destravadas por E-F3-001-04, sem aceite de implementação dessas etapas.  
**Champion:** João Paulo  
**Prazo de referência:** sem trava de prazo (decisão de 07/10/2026); a referência inicial de 30/09/2026 venceu.  
**Atualizado em:** 30/10/2026 pelo Maestro — leitura real da Meta executada e CA-3-16 atestado; sem alteração de código nesta atualização.  
**Regra:** uma task por vez; cada conclusão exige as provas da SPEC e o teste humano do Champion.

## Esta fase em linguagem simples

Na Fase 3 o sistema passa a **mostrar** o caminho de cada demanda. Ao final, vocês vão ter no Skip (projeto **"repositório adapta"**):

- **Funil** — um quadro em colunas (Kanban): cada cartão é uma empresa/demanda e mostra de onde veio (inbound ou outbound, canal, campanha), o que ele
- **Campanhas** — as campanhas Meta Ads (ativas ou pausadas, gasto, resultados), atualizadas todo dia por uma automação no Maestro, com aviso se a co
- **Gargalos** — quantas demandas há em cada etapa e onde estão travando.
- **Passagens pendentes** — quando o comercial passa uma demanda para o R&S ou a alocação, para quem foi e se foi aceita.
- **Área do DHO** — separada, só para o DHO.

Enquanto a política de dados (LGPD) não for aprovada pela direção, as telas mostram **dados de teste**, marcados com o selo TESTE.

**Como o Maestro vai trabalhar com vocês:** antes de cada task ele explica o que vai criar, combina cada passo ("vamos criar isso no Skip agora"), pe

## Ordem recomendada (sequência de referência; situação vigente abaixo)

F3-T01 → F3-T09 → F3-T10 → F3-T11 → F3-T12 → F3-T13 → F3-T02 → F3-T03 → F3-T04 → F3-T05 → F3-T06 → F3-T07 → F3-T08

## Situação vigente e retomada

**Consolidação documental de 29/10/2026:** E-F3-001-04 vigente; D1–D6 decididas. E2-coleções/E5/E6 não aguardam nova decisão D4/D5/D6. A implementação pendente continua sujeita à autorização do Champion, às provas da SPEC e ao teste humano. Nenhuma task foi concluída por esta correção; nenhum código ou dado foi alterado.

**Atualização de 30/10/2026:** leitura real da Meta executada (10 campanhas, 70 linhas, janela 22–28/10) pela fonte Graph API direta; CA-3-16 atestado pelo owner contra o Gerenciador. Registro: `05_entregas/campanhas-f3.md`; aceite: `06_notas/aceite-graph-api-f3-t12-2026-10-30.md`.

F3-T12 continua ativa até a confirmação do teste humano: a leitura real foi executada e o CA-3-16 foi atestado pelo owner em 30/10 (fonte Graph API direta). Depois, concluir ou registrar pausa segura explícita antes de retomar F3-T01. A ordem abaixo não reabre F3-T09 nem autoriza duas tasks simultâneas. F3-T10 segue dependente de F3-T01 integralmente aceita.

## Jornada operacional

- [/] Materializar campos de proposta/vaga, retorno e escalonamento no pipeline com massa sintética @"João Paulo" #projeto
  > F3-T01 · SPEC F3-001 · CA-3-01/02/04 (E-F3-001-01). Criar fixtures inbound/outbound; percorrer o pipeline até proposta/vaga com retorno; exercitar escalonamento de oportunidade parada; demonstrar hierarquia Demanda→Propostas→Vagas; registrar captura/export e log de transições. Parar se exigir dado real, dedupe por inferência ou integração externa.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: cada demanda de teste ganha propostas, vagas, retorno da operação e aviso quando fica parada.
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
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: lista "Passagens pendentes" — para quem a demanda foi passada, prazo para aceitar e se foi aceita, recusada ou venceu.
- [ ] Provar aceite, recusa, SLA vencido, sem resposta e falha de acesso com matriz nominal @"João Paulo" #projeto
  > F3-T06 · SPEC F3-003 · CA-3-06..08. Executar bateria humana completa, incluindo falha segura sem retry automático, matriz RACI e log de eventos. Depende de F3-T05 aceita e B3-RACI-01 fechado.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: o teste com os nomes reais da matriz, cada situação mostrando o que aconteceu na tela.
- [ ] Materializar objeto separado de sinal de conta do DHO com lista versionada e permissões fail-closed @"João Paulo" #projeto
  > F3-T07 · SPEC F3-004 · CA-3-09. Criar objeto inativo com fixture, permissões server-side e prova negativa de leitura comercial; registrar decisão C-06. Depende de F3-T01 aceita; dado real permanece bloqueado por B3-DHO-01.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: área separada, só do DHO, com um sinal da conta em dado de teste; o comercial não vê essa área.
- [ ] Provar isolamento, lista fechada, encaminhamento humano e fail-closed sem política @"João Paulo" #projeto
  > F3-T08 · SPEC F3-004 · CA-3-09/10. Provar sinal autorizado em fixture, isolamento, duplicata, rollback e encaminhamento humano; nenhum sinal fora
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a prova de que o comercial não vê o sinal e que o sinal vira encaminhamen
- [x] Diagnosticar no Maestro as automações ativas, o destino dos leads da landing page e as campanhas Meta, sem alterar nada @"João Paulo" [interno]
  > F3-T09 · SPEC F3-006 · CA-3-15. Somente leitura: listar automações e destinos, confirmar a parada da automação do "Hub de gestão Meta Ads", achar
  > Onde executar: Maestro (somente leitura). O que você vai ver: um relatório curto dizendo quais automações estão ligadas, para onde cada uma manda
- [ ] Construir a tela Funil (Kanban) com cartões, origem, filtros e Jornada usando os dados de teste @"João Paulo" [interno] #projeto
  > F3-T10 · SPEC F3-005 · CA-3-13. Colunas por etapa, cartão com origem/canal/campanha/serviço/área/dono/próxima ação, selo TESTE, filtros e Jornada
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: tela "Funil" em colunas por etapa; cada cartão mostra de onde a demanda v
- [ ] Provar o Funil: origem desconhecida, arrastar sem evidência, contagens e regressão do painel @"João Paulo" [interno] #projeto
  > F3-T11 · SPEC F3-005 · CA-3-13/14. RED/GREEN/regressão da SPEC. Depende de F3-T10 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a prova de que o Funil mostra "desconhecido" quando falta origem, recusa 
- [ ] Criar a tela Campanhas e a automação diária do Maestro que lê a Meta e grava só no projeto oficial @"João Paulo" [interno] #projeto
  > F3-T12 · SPEC F3-006 · CA-3-16. Coleções campanhas_meta/conexao_meta, página Campanhas, ficha da automação e execução manual conferida com o Gere
  > Onde executar: Maestro (automação agendada) → Skip "repositório adapta". O que você vai ver: tela "Campanhas" com cada campanha Meta: ativa ou pau
- [ ] Provar conexão expirada, reexecução sem duplicar, destino único e nenhuma escrita na Meta @"João Paulo" [interno] #projeto
  > F3-T13 · SPEC F3-006 · CA-3-16/17. RED/GREEN/regressão da SPEC. Depende de F3-T12 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a prova de que, se a conexão cair, aparece um aviso na tela e chega um e
