# Fase 3 — Tarefas

<!-- fase-format:2 -->

**Plano:** TG Mais Serviços de Tecnologia e RH LTDA · `6b09d189`  
**Estado:** Fase 3 em execução; **6/13 tasks concluídas** — F3-T09 (com ressalvas), F3-T13 (04/11), F3-T12 (03/11), F3-T01 (04/11), F3-T10 (11/11) e **F3-T03 CONCLUÍDA em 16/11/2026** (tela Gargalos em 5 entregas homologadas: etapa, tempo, prazo vencido, retorno e pendências com captura).  
**Champion:** João Paulo  
**Atualizado em:** 16/11/2026 pelo Maestro — F3-T03 concluída após homologação da Entrega 5 (v0.0.208, arquivo de captura conferido 17/17). F3-T04 liberada.  
**Regra:** uma task por vez; cada conclusão exige as provas da SPEC e o teste humano do Champion.

## Esta fase em linguagem simples

Na Fase 3 o sistema passa a **mostrar** o caminho de cada demanda. Ao final, vocês vão ter no Skip (projeto **"repositório adapta"**):

- **Funil** — um quadro em colunas (Kanban): cada cartão é uma empresa/demanda e mostra de onde veio (inbound ou outbound, canal, campanha), o que ela quer (R&S ou TMO), quem é o dono e a próxima ação. Clicando, aparece a história completa. Arrastando, muda de etapa — com o motivo registrado. **✅ entregue (F3-T10)**
- **Campanhas** — as campanhas Meta Ads (ativas ou pausadas, gasto, resultados), atualizadas todo dia por uma automação no Maestro, com aviso se a conexão cair. **✅ entregue (F3-T12/T13)**
- **Gargalos** — quantas demandas há em cada etapa, quanto tempo ficam em cada uma, o que passou do prazo, o que voltou das propostas e vagas, e o que ainda depende de decisão. **✅ entregue (F3-T03)**
- **Passagens pendentes** — quando o comercial passa uma demanda para o R&S ou a alocação, para quem foi e se foi aceita. **⏳ pendente (F3-T05/T06)**
- **Área do DHO** — separada, só para o DHO. **⏳ pendente (F3-T07/T08)**

Enquanto a política de dados (LGPD) não for aprovada pela direção, as telas mostram **dados de teste**, marcados com o selo TESTE.

**Como o Maestro vai trabalhar com vocês:** antes de cada task ele explica o que vai criar, combina cada passo ("vamos criar isso no Skip agora"), pede para vocês darem uma olhada no Skip depois de cada passo e fecha com um resumo do que já dá para ver e qual é a próxima task.

## Ordem recomendada (sequência de referência; situação vigente abaixo)

F3-T01 → F3-T09 → F3-T10 → F3-T11 → F3-T12 → F3-T13 → F3-T02 → F3-T03 → F3-T04 → F3-T05 → F3-T06 → F3-T07 → F3-T08

## Situação vigente e retomada

**Consolidação documental de 29/10/2026:** E-F3-001-04 vigente; D1–D6 decididas. A implementação pendente continua sujeita à autorização do Champion, às provas da SPEC e ao teste humano.

**Atualização de 03/11/2026 — F3-T12 CONCLUÍDA:** o owner conferiu a tela `/campanhas` recarregada e os sete números do último retrato batem com a Graph API. Três defeitos reais corrigidos: `normalizarStatus`, ausência de porta de entrada própria e o campo `resultados` somando os 13 tipos de `actions`. Registro: `05_entregas/campanhas-f3.md`; aceite: `06_notas/aceite-graph-api-f3-t12-2026-10-30.md`.

**Atualização de 04/11/2026 — F3-T01 CONCLUÍDA:** coleções `propostas` e `vagas` (migração 0026), histórico append-only `pipeline_log` (0027), campos de recuperação (0028) e os três cenários sintéticos (0029). Regras provadas: filho sem demanda válida é rejeitado; empresa não existe no filho; filho não tem estado de pipeline; "Substituição encerrada" exige motivo; recuperação limitada a 3 tentativas em 90 dias corridos da primeira tentativa formal humana. Tela `/pipeline-t01`. Teste humano aprovado em 04/11. Registro em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`.

**Atualização de 04/11/2026 — F3-T13 CONCLUÍDA:** provas do CA-3-17 com falha real. E-mail enviado e no log, faixa na tela, 130 linhas anteriores intactas, reexecução 0 criadas / 70 atualizadas, 0 escrita na Meta. Três defeitos corrigidos: ensaio sujava o estado; hora crua em UTC; token expirado volta 400/190 e não 401. Homologação: "Aprovado — pode concluir a T13". Registro em `06_notas/changelog-2026-11-04-f3-t13-concluida.md`.

**Atualização de 11/11/2026 — F3-T10 CONCLUÍDA:** tela Funil em cinco etapas, cada uma homologada. A prova final foi do owner: arrastou "Serviços Delta Ltda." de Oportunidade para Proposta e o sistema gravou a transição. Quatro defeitos reais corrigidos: zero falso sem sessão; contador fora do filtro; hora crua em UTC; tela em branco na E5. Registro em `06_notas/changelog-2026-11-11-f3-t10-concluida.md`.

**Atualização de 16/11/2026 — F3-T03 CONCLUÍDA:** tela `/gargalos` entregue em cinco etapas homologadas. **E1** visão por etapa (v0.0.198, homologada em 13/11); **E2** tempo na etapa (v0.0.200); **E3** demandas com prazo vencido (v0.0.202); **E4** retorno de propostas e vagas (v0.0.203); **E5** pendências de governança e captura da leitura (v0.0.208). **Sete defeitos reais corrigidos no caminho** — cinco apontados pelo owner olhando a tela ou o arquivo de captura: 4 etapas omitidas na visão de tempo; "hoje" em UTC em vez de Brasília; prazo da vaga aberta usando o prazo da demanda; dois textos desatualizados prometendo entregas futuras; "mediana sem mediana"; e serviço/área faltando no arquivo de captura. Nenhuma coleção, campo, índice ou migration de estrutura nesta task — a entrega é somente leitura. Registro completo em `artifacts/ENCERRAMENTO-T03-2026-11-16.md`.

**Próxima:** F3-T04 (provas de leitura de gargalo), liberada em 16/11/2026 com a conclusão da F3-T03. Exige autorização do Champion. A F3-T11 (provas do Funil) segue liberada desde 11/11 e também aguarda autorização.

## Jornada operacional

- [x] Materializar campos de proposta/vaga, retorno e escalonamento no pipeline com massa sintética @"João Paulo" #projeto
  > **Concluída em 04/11/2026** — teste humano do owner aprovado. Evidência em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`.
- [ ] Provar jornada dupla, retorno da operação, escalonamento e equivalência entre duas pessoas @"João Paulo" #projeto
  > F3-T02 · SPEC F3-001 · CA-3-01..04. **BLOQUEADA (B3-RACI-01)** — falta a matriz de responsáveis.
- [x] Materializar visões de gargalo, tempos e retorno no dashboard com massa sintética @"João Paulo" #projeto
  > **Concluída em 16/11/2026** — 5 entregas homologadas (v0.0.198 · 0.0.200 · 0.0.202 · 0.0.203 · 0.0.208). Evidência em `artifacts/ENCERRAMENTO-T03-2026-11-16.md`.
- [ ] Provar leitura de gargalo, bloqueio de KPI sem alvo e reprodução das contagens @"João Paulo" #projeto
  > F3-T04 · SPEC F3-002 · CA-3-11/12. **LIBERADA em 16/11/2026** — exige autorização do Champion.
- [ ] Materializar registro de handoff com SLA, escalonamento e recuperação com massa sintética @"João Paulo" #projeto
  > F3-T05 · SPEC F3-003 · CA-3-06. **LIBERADA** (F3-T01 concluída em 04/11) — exige autorização do Champion.
- [ ] Provar aceite, recusa, SLA vencido, sem resposta e falha de acesso com matriz nominal @"João Paulo" #projeto
  > F3-T06 · SPEC F3-003 · CA-3-06..08. **BLOQUEADA (F3-T05, B3-RACI-01)**.
- [ ] Materializar objeto separado de sinal de conta do DHO com lista versionada e permissões fail-closed @"João Paulo" #projeto
  > F3-T07 · SPEC F3-004 · CA-3-09. **BLOQUEADA (C-06)**.
- [ ] Provar isolamento, lista fechada, encaminhamento humano e fail-closed sem política @"João Paulo" #projeto
  > F3-T08 · SPEC F3-004 · CA-3-09/10. **BLOQUEADA (F3-T07)**.
- [x] Diagnosticar no Maestro as automações ativas, o destino dos leads da landing page e as campanhas Meta @"João Paulo" [interno] #projeto
  > **Concluída em 24/10/2026** com ressalvas.
- [x] Construir a tela Funil (Kanban) com cartões, origem, filtros e Jornada @"João Paulo" [interno] #projeto
  > **Concluída em 11/11/2026** — cinco etapas homologadas pelo owner.
- [ ] Provar o Funil: origem desconhecida, arrastar sem evidência, contagens e regressão do painel @"João Paulo" [interno] #projeto
  > F3-T11 · SPEC F3-005 · CA-3-13/14. **LIBERADA desde 11/11** — exige autorização do Champion.
- [x] Criar a tela Campanhas e a automação diária do Maestro @"João Paulo" [interno] #projeto
  > **Concluída em 03/11/2026**.
- [x] Provar conexão expirada, reexecução sem duplicar, destino único e nenhuma escrita na Meta @"João Paulo" [interno] #projeto
  > **Concluída em 04/11/2026** — homologada pelo owner.

## Tasks

| ID | Leva | Task | Onde executar | O que você vai ver | Dono | SPEC | Critério | Subseção exata da SPEC | Recorte da prova | Evidência esperada | Pré-condições | Ponto de parada | Status |
|---|---:|---|---|---|---|---|---|---|---|---|---|---|---|
| F3-T01 | 1 | Materializar campos de proposta/vaga, retorno e escalonamento no pipeline com massa sintética. | Maestro → Skip "repositório adapta" | Cada demanda de teste ganha propostas, vagas, retorno da operação e aviso quando fica parada. | João Paulo | F3-001 | CA-3-01, CA-3-02, CA-3-04 (E-F3-001-01) | Dados e integrações; Fluxo e regras; Checklist | Fixtures inbound/outbound, proposta/vaga, retorno e escalonamento; hierarquia Demanda→Propostas→Vagas. | Captura/export da jornada e log de transições. | F2 9/9 aceita; fixtures sintéticos; pipeline F1/F2 preservado; E-F3-001-01 vigente. | Parar se exigir dado real, dedupe por inferência ou integração externa. | CONCLUÍDA 04/11 — teste humano do owner aprovado |
| F3-T09 | 1 | Diagnosticar no Maestro as automações ativas, o destino dos leads da landing page e as campanhas Meta, sem alterar nada. | Maestro (somente leitura) | Um relatório curto: automações ligadas, destinos, leads do formulário e campanhas ativas. | João Paulo | F3-006 | CA-3-15 | Fluxo e regras (passos 1–2); Checklist | Perguntas ao Maestro e conferência no Skip; nenhuma criação ou alteração. | `diagnostico-maestro-f3-t09.md`. | Conectores Meta e Skip ativos no Maestro. | Parar se encontrar destino de leads fora do projeto oficial. | CONCLUÍDA 24/10 com ressalvas |
| F3-T10 | 2 | Construir a tela Funil (Kanban) com cartões, origem, filtros e Jornada usando os dados de teste. | Maestro → Skip "repositório adapta" | Tela "Funil" em colunas por etapa; cartão com origem, serviço, dono e próxima ação; clicando, a história completa. | João Paulo | F3-005 | CA-3-13 | Dados e integrações; Fluxo e regras; Checklist | Colunas, cartões, filtros e Jornada com os fixtures da F3-T01. | Capturas do Funil e da Jornada + comparação fonte×tela. | F3-T01 aceita por teste humano. | Parar se precisar de campo inexistente ou dado de contato real. | CONCLUÍDA em 11/11/2026 — 5 etapas homologadas; arrastar com evidência provado pelo owner |
| F3-T11 | 3 | Provar o Funil: origem desconhecida, arrastar sem evidência, contagens e regressão do painel. | Maestro → Skip "repositório adapta" | A prova de que o Funil mostra "desconhecido" quando falta origem, recusa mover sem evidência e tem as contagens da fonte. | João Paulo | F3-005 | CA-3-13, CA-3-14 | Critérios de aceite; TDD da SPEC | RED/GREEN/regressão da SPEC-F3-005. | Bateria humana + log de transições + comparação fonte×tela. | F3-T10 aceita por teste humano (atingido 11/11). | Parar se o cartão mover sem evidência ou a contagem divergir. | LIBERADA desde 11/11 — exige autorização do Champion |
| F3-T12 | 2 | Criar a tela Campanhas e a automação diária do Maestro que lê a Meta e grava só no projeto oficial. | Maestro (automação agendada) → Skip "repositório adapta" | Tela "Campanhas" com cada campanha Meta, orçamento, gasto, impressões, cliques e resultados. | João Paulo | F3-006 | CA-3-16 | Dados e integrações; Fluxo e regras (passo 3); Checklist | Coleções `campanhas_meta`/`conexao_meta`, página e ficha da automação. | Ficha da automação + capturas tela × Gerenciador. | F3-T09 aceita; B3-META-LEITOR resolvido. | Parar se exigir escrita na Meta ou segredo no chat. | CONCLUÍDA 03/11 — CA-3-16 atestado pelo owner |
| F3-T13 | 3 | Provar conexão expirada, reexecução sem duplicar, destino único e nenhuma escrita na Meta. | Maestro → Skip "repositório adapta" | A prova de que, se a conexão cair, aparece aviso na tela e chega e-mail, sem apagar os números de antes. | João Paulo | F3-006 | CA-3-16, CA-3-17 | Critérios de aceite; TDD da SPEC | RED/GREEN/regressão da SPEC-F3-006. | Log das execuções + e-mail recebido + bateria humana. | F3-T12 aceita por teste humano. | Parar se houver escrita na Meta, duplicação ou gravação fora do projeto. | CONCLUÍDA em 04/11/2026 — homologada pelo owner |
| F3-T02 | 2 | Provar jornada dupla, retorno da operação, escalonamento e equivalência entre duas pessoas. | Maestro → Skip "repositório adapta" | Roteiro em que duas pessoas classificam os mesmos casos e chegam à mesma etapa. | João Paulo | F3-001 | CA-3-01..04 | Critérios de aceite; TDD | RED/GREEN/regressão, replay e bateria de equivalência. | Bateria humana + log + comparação fonte×painel. | F3-T01 aceita; B3-RACI-01 para a prova de handoff. | Parar sem critério de classificação ou matriz nominal. | BLOQUEADA (B3-RACI-01) |
| F3-T03 | 3 | Materializar visões de gargalo, tempos e retorno no dashboard com massa sintética. | Maestro → Skip "repositório adapta" | Tela "Gargalos": demandas por etapa, tempo em cada etapa, o que passou do prazo, retorno de propostas e vagas, pendências e captura. | João Paulo | F3-002 | CA-3-11 (E-F3-002-01) | Dados e integrações; Fluxo e regras; Checklist | Visões por estado, gargalo, tempos e retorno; KPI contra alvo bloqueado. | Captura/export com contagens reproduzíveis. | F3-T01 aceita (atingido 04/11). | Parar se a contagem não reproduzir a fonte ou se pedirem KPI sem alvo. | CONCLUÍDA 16/11/2026 — 5 entregas homologadas (v0.0.198 · 0.0.200 · 0.0.202 · 0.0.203 · 0.0.208) |
| F3-T04 | 4 | Provar leitura de gargalo, bloqueio de KPI sem alvo e reprodução das contagens. | Maestro → Skip "repositório adapta" | A direção abre a tela e diz onde a demanda trava; números iguais ao Funil; meta como "pendente de decisão". | João Paulo | F3-002 | CA-3-11/12 (E-F3-002-01) | Critérios de aceite; TDD | Leitura completa, volume×qualidade, pendência G-001 e regressão F1/F2. | Bateria humana + comparação fonte×dashboard. | F3-T03 aceita. | Parar se esconder lacuna ou misturar volume com qualidade. | LIBERADA 16/11/2026 — exige autorização do Champion |
| F3-T05 | 3 | Materializar registro de handoff com SLA, escalonamento e recuperação com massa sintética. | Maestro → Skip "repositório adapta" | Lista "Passagens pendentes": para quem a demanda foi passada, prazo para aceitar e se foi aceita, recusada ou venceu. | João Paulo | F3-003 | CA-3-06 | Dados e integrações; Fluxo e regras; Checklist | HO-F3-001 com destinatário, prazo de aceite, aceite, recusa e expiração. | Captura/export do registro e estados. | F3-T01 aceita; papéis provisórios registrados. | Parar se exigir disparo externo ou encerramento sem responsável. | LIBERADA (F3-T01 concluída em 04/11) |
| F3-T06 | 5 | Provar aceite, recusa, SLA vencido, sem resposta e falha de acesso com matriz nominal. | Maestro → Skip "repositório adapta" | O teste com os nomes reais da matriz: cada situação mostrando o que aconteceu na tela. | João Paulo | F3-003 | CA-3-06..08 | Critérios de aceite; TDD | Bateria humana completa e falha segura sem retry automático. | Matriz RACI + log de eventos + evidências. | F3-T05 aceita; B3-RACI-01 fechado. | Parar sem matriz nominal ou se pedirem retry automático. | BLOQUEADA (F3-T05, B3-RACI-01) |
| F3-T07 | 4 | Materializar objeto separado de sinal de conta do DHO com lista versionada e permissões fail-closed. | Maestro → Skip "repositório adapta" | Área separada, só do DHO, com um sinal da conta em dado de teste; o comercial não vê. | João Paulo | F3-004 | CA-3-09 | Dados e integrações; Fluxo e regras; Checklist | Objeto inativo com fixture, permissões server-side e prova negativa. | Captura do objeto + prova negativa + decisão C-06. | F3-T01 aceita; decisão C-06 registrada. | Parar se pedirem dado real sem política ou exposição ao comercial. | BLOQUEADA (C-06) |
| F3-T08 | 6 | Provar isolamento, lista fechada, encaminhamento humano e fail-closed sem política. | Maestro → Skip "repositório adapta" | A prova de que o comercial não vê o sinal e que o sinal vira encaminhamento para uma pessoa. | João Paulo | F3-004 | CA-3-09/10 | Critérios de aceite; TDD | Sinal em fixture, isolamento, duplicata, rollback e encaminhamento humano. | Bateria humana + lista versionada + decisão de uso. | F3-T07 aceita. | Parar se gerar ação sem decisor ou aparecer no pipeline comercial. | BLOQUEADA (F3-T07) |

## Bloqueios transversais

- **B3-META-01:** a F3 só lê métricas de campanha Meta pela automação da SPEC-F3-006; sem dado pessoal e sem escrita na Meta.
- **B3-META-LEITOR:** resolvido pelo aceite de 28/10, reafirmado em 30/10 para Graph API direta; permanece somente leitura.
- **B3-LGPD-01:** dado real de contato só aparece após política aprovada pela direção; até lá, selo TESTE.
- **B3-ID-01:** identidade por fonte; sem dedupe por inferência.
- **B3-MET-01:** alvo, fórmula, período e lead qualificado antes de KPI de resultado. **Efeito visível:** a tela Gargalos declara, em "Pendências de governança", que nenhum KPI contra alvo é exibido por causa deste bloqueio.
- **B3-RACI-01:** matriz nominal antes da prova F3-T06 e da prova de handoff da F3-T02. **Efeito visível:** a tela Gargalos declara que mostra o dono do registro e não atribui responsável.
- **B3-DHO-01:** lista de sinais e política antes de dado real de sinal; F3-004 permanece fail-closed com fixtures.
- **D4/D5/D6:** decididas pela E-F3-001-04, implementadas e provadas na F3-T01 (04/11). Não são mais bloqueio documental.
