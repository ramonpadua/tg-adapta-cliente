# Fase 3 — Pipeline, indicadores e campanhas

**Fase:** 3 de 5
**Estado:** Fase 3 em execução; 4/13 tasks concluídas — F3-T09 (com ressalvas), **F3-T13 CONCLUÍDA em 04/11/2026** (provas de falha executadas com falha real: e-mail enviado e confirmado no log, faixa de aviso na tela, retrato anterior intacto, zero escrita na Meta) e **F3-T12 CONCLUÍDA em 03/11/2026** (tela Campanhas lendo a Meta por Graph API direta; leitura real comprovada; CA-3-16 atestado pelo owner; conferência final da tela contra a API campo a campo). **F3-T01 CONCLUÍDA em 04/11/2026**: propostas/vagas, histórico append-only, retorno da operação e recuperação 3/90 implementados e testados pelo owner. E2 origem_dado aceita em 23/10 e preservada.

**Atualizado em:** 04/11/2026 pelo Maestro — F3-T13 concluída após teste humano do owner; quatro defeitos reais encontrados e corrigidos durante a implementação (registro em `06_notas/changelog-2026-11-04-f3-t01-concluida.md` e `06_notas/changelog-2026-11-04-f3-t13-concluida.md`).

---

## Ordem de execução

F3-T01 → F3-T09 → F3-T10 → F3-T11 → F3-T12 → F3-T13 → F3-T02 → F3-T03 → F3-T04 → F3-T05 → F3-T06 → F3-T07 → F3-T08

## Situação vigente e retomada

**Consolidação documental de 29/10/2026:** E-F3-001-04 vigente; D1–D6 decididas. E2-coleções/E5/E6 não aguardam nova decisão D4/D5/D6. A implementação pendente continua sujeita à autorização do Champion, às provas da SPEC e ao teste humano. Nenhuma task foi concluída por esta correção; nenhum código ou dado foi alterado.

**Atualização de 04/11/2026 — F3-T01 CONCLUÍDA:** implementadas as coleções `propostas` e `vagas` ligadas à demanda (migração 0026), o histórico append-only `pipeline_log` (0027), os campos de recuperação comercial em `demandas` (0028) e os três cenários sintéticos PIPE-F3-IN-001/OUT-001/STUCK-001 (0029). Regras provadas: filho sem demanda válida é rejeitado; empresa não existe no filho; proposta/vaga não têm estado de pipeline; "Substituição encerrada" exige motivo "reposto" ou "encerrada sem reposição" e não encerra vaga nem demanda; recuperação limitada a 3 tentativas em 90 dias corridos contados da **primeira tentativa formal humana**, sem reinício por novas tentativas, mudanças de etapa ou automações (decisão do Champion de 04/11/2026). Tela `/pipeline-t01` mostra propostas, vagas, retorno e a jornada reconstruída do histórico. Teste humano aprovado pelo owner em 04/11 ("perfeito. vi aqui. ok tudo certo"). Registro completo em `06_notas/changelog-2026-11-04-f3-t01-concluida.md`.

Com a T01 concluída, a próxima é a **F3-T13**, que exige autorização do Champion. A ordem abaixo não reabre F3-T09 nem autoriza duas tasks simultâneas. F3-T10 segue dependente de F3-T01 integralmente aceita — requisito atingido em 04/11.

## Jornada operacional

- [x] Materializar campos de proposta/vaga, retorno e escalonamento no pipeline com massa sintética @"João Paulo" #projeto
  > F3-T01 · SPEC F3-001 · CA-3-01/02/04 (E-F3-001-01). Criar fixtures inbound/outbound; percorrer o pipeline até proposta/vaga com retorno; exercitar escalonamento de oportunidade parada; demonstrar hierarquia Demanda→Propostas→Vagas; registrar captura/export e log de transições. Parar se exigir dado real, dedupe por inferência ou integração externa.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: o Funil mostrando a demanda, as propostas ligadas a ela e as vagas abertas; o histórico de cada mudança de etapa; a devolutiva da operação registrada; e o alerta quando a oportunidade passa do prazo.
- [x] Executar as baterias de equivalência e reprodutibilidade com massa sintética @"João Paulo" #projeto
  > F3-T02 · SPEC F3-001 · CA-3-01..04. Executar RED/GREEN/regressão, replay, contagens pela fonte, hierarquia Demanda→Propostas→Vagas e bateria de equivalência. Depende de F3-T01 aceita e da matriz RACI nominal para a prova de handoff.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: um roteiro em que duas pessoas classificam os mesmos casos e chegam à mesma etapa, com "passou / não passou" por caso.
- [x] Materializar visões de gargalo, tempos e retorno no dashboard com massa sintética @"João Paulo" #projeto
  > F3-T03 · SPEC F3-002 · CA-3-11 (E-F3-002-01). Criar visões por estado, gargalo, tempos e retorno; manter o KPI contra alvo bloqueado e a pendência B3-MET-01 visível. Depende de F3-T01 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a tela "Gargalos": quantas demandas há em cada etapa, quanto tempo levam entre etapas e quais estão paradas além do prazo.
- [ ] Provar leitura de gargalo, bloqueio de KPI sem alvo e reprodução das contagens @"João Paulo" #projeto
  > F3-T04 · SPEC F3-002 · CA-3-11/12. Leitura completa, volume×qualidade, pendência G-001 e regressão F1/F2. Depende de F3-T03 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a direção abrindo a tela e dizendo onde a demanda trava; números iguais ao Funil; meta como "pendente de decisão".
- [ ] Materializar registro de handoff com SLA, escalonamento e recuperação com massa sintética @"João Paulo" #projeto
  > F3-T05 · SPEC F3-003 · CA-3-06. Criar HO-F3-001 com destinatário, SLA e estado; exercitar aceite, recusa e expiração com papéis provisórios registrados. Depende de F3-T01 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a lista "Passagens pendentes": para quem a demanda foi passada, prazo para aceitar e se foi aceita, recusada ou venceu.
- [ ] Criar o objeto inativo com prova negativa de leitura comercial @"João Paulo" #projeto
  > F3-T07 · SPEC F3-004 · CA-3-09. Criar objeto inativo com fixture, permissões server-side e prova negativa de leitura comercial; registrar decisão C-06. Depende de F3-T01 aceita; dado real permanece bloqueado por B3-DHO-01.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: o registro arquivado recusando acesso comercial, com a prova de que não aparece para quem não deve ver.
- [x] Entregar o diagnóstico de automações, destinos e campanhas @"João Paulo" [interno] #projeto
  > F3-T09 · SPEC F3-006 · CA-3-15. Relatório sem tocar em automação, banco ou campanha. Aceita em 23/10 com ressalvas.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: o retrato de tudo que roda hoje, para onde manda e o que está parado.
- [ ] Materializar as visões de pipeline e o alerta de competitividade @"João Paulo" #projeto
  > F3-T11 · SPEC F3-005 · CA-3-13/14. Depende de F3-T01 aceita e da decisão sobre concorrentes. Hermes: sem fonte de concorrente, mostrar "pendente de fonte" e não número.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a tela "Concorrência": quem está contratando perto de você e o alerta quando um cliente abre vaga parecida com a sua.
- [ ] Criar a tela Campanhas e a automação diária do Maestro que lê a Meta e grava só no projeto oficial @"João Paulo" [interno] #projeto
  > F3-T12 · SPEC F3-006 · CA-3-16. Coleções campanhas_meta/conexao_meta, página Campanhas, ficha da automação e execução manual conferida com o Gerenciador de Anúncios. Depende de F3-T09 aceita e B3-META-LEITOR resolvido.
  > Onde executar: Maestro (automação agendada) → Skip "repositório adapta". O que você vai ver: tela "Campanhas" com cada campanha Meta — ativa ou pausada, orçamento, gasto, impressões, cliques, resultados e hora da atualização.
- [x] Provar conexão expirada, reexecução sem duplicar, destino único e nenhuma escrita na Meta @"João Paulo" [interno] #projeto
  > F3-T13 · SPEC F3-006 · CA-3-16/17. RED/GREEN/regressão da SPEC. Depende de F3-T12 aceita.
  > Onde executar: Maestro → Skip "repositório adapta". O que você vai ver: a prova de que, se a conexão cair, aparece um aviso na tela e chega um e-mail, sem apagar os números de antes; e que nada é escrito na Meta.

## Tasks

| Task | Ordem | Escopo | Onde executar | O que você vai ver | Champion | SPEC | Critérios | Passos | Provas | Depende de | Parar se | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| F3-T09 | 1 | Entregar diagnóstico de automações, destinos e campanhas | Maestro → Skip "repositório adapta" | O retrato de tudo que roda hoje, para onde manda e o que está parado | João Paulo | F3-006 | CA-3-15 | Critérios de aceite; Checklist | Relatório versionado + conferência humana | — | Parar se precisar alterar automação, banco ou campanha | CONCLUÍDA (com ressalvas) |
| F3-T10 | 2 | Provar a parada da automação duplicada e a execução manual do substituto | Maestro → Skip "repositório adapta" | A automação antiga desligada e a nova rodando uma vez, com o mesmo resultado | João Paulo | F3-006 | CA-3-16/17 | RED/GREEN/regressão | Log das execuções + captura | F3-T01 aceita (atingido 04/11) | Parar se a antiga for desligada sem substituto aceito | BLOQUEADA (aguarda autorização) |
| F3-T11 | 3 | Materializar visões de pipeline e alerta de competitividade | Maestro → Skip "repositório adapta" | A tela "Concorrência" com quem contrata perto e alerta de vaga parecida | João Paulo | F3-005 | CA-3-13/14 | Dados e integrações; Fluxo e regras | Captura/export com fontes declaradas | F3-T01 aceita (atingido 04/11) | Parar se inventar número sem fonte | LIBERADA |
| F3-T12 | 2 | Criar a tela Campanhas e a automação diária do Maestro que lê a Meta e grava só no projeto oficial | Maestro (automação agendada) → Skip "repositório adapta" | Tela "Campanhas" com cada campanha Meta — ativa ou pausada, orçamento, gasto, impressões, cliques, resultados e hora da atualização | João Paulo | F3-006 | CA-3-16 | Dados e integrações; Fluxo e regras (passo 3); Checklist | Ficha da automação + capturas tela × Gerenciador | F3-T09 aceita; B3-META-LEITOR resolvido | Parar se ler dado pessoal ou escrever na Meta | CONCLUÍDA em 03/11/2026 |
| F3-T13 | 3 | Provar conexão expirada, reexecução sem duplicar, destino único e nenhuma escrita na Meta. | Maestro → Skip "repositório adapta" | A prova de que, se a conexão cair, aparece um aviso na tela e chega um e-mail, sem apagar os números de antes; e que nada é escrito na Meta. | João Paulo | F3-006 | CA-3-16, CA-3-17 | Critérios de aceite; TDD da SPEC | RED/GREEN/regressão da SPEC-F3-006. | Log das execuções + e-mail recebido + bateria humana. | F3-T12 aceita por teste humano (atingido 03/11). | Parar se houver escrita na Meta, duplicação ou gravação fora do projeto oficial. | CONCLUÍDA em 04/11/2026 — homologada pelo owner ("Aprovado — pode concluir a T13") |
| F3-T02 | 2 | Provar jornada dupla, retorno da operação, escalonamento e equivalência entre duas pessoas. | Maestro → Skip "repositório adapta" | Um roteiro em que duas pessoas classificam os mesmos casos e chegam à mesma etapa, com "passou / não passou" por caso. | João Paulo | F3-001 | CA-3-01..04 | Critérios de aceite; TDD | RED/GREEN/regressão, replay e bateria de equivalência. | Bateria humana + log + comparação fonte×painel. | F3-T01 aceita (atingido 04/11); B3-RACI-01 para a prova de handoff. | Parar sem critério de classificação ou matriz nominal. | BLOQUEADA (B3-RACI-01) |
| F3-T03 | 3 | Materializar visões de gargalo, tempos e retorno no dashboard com massa sintética. | Maestro → Skip "repositório adapta" | Tela "Gargalos": quantas demandas há em cada etapa, quanto tempo levam entre etapas e quais estão paradas além do prazo. | João Paulo | F3-002 | CA-3-11 (E-F3-002-01) | Dados e integrações; Fluxo e regras; Checklist | Visões por estado, gargalo, tempos e retorno; KPI contra alvo bloqueado. | Captura/export com contagens reproduzíveis. | F3-T01 aceita (atingido 04/11). | Parar se a contagem não reproduzir a fonte ou se pedirem KPI sem alvo. | LIBERADA (F3-T01 concluída em 04/11) |
| F3-T04 | 4 | Provar leitura de gargalo, bloqueio de KPI sem alvo e reprodução das contagens. | Maestro → Skip "repositório adapta" | A direção abre a tela e diz onde a demanda trava; números iguais ao Funil; meta como "pendente de decisão". | João Paulo | F3-002 | CA-3-11/12 (E-F3-002-01) | Critérios de aceite; TDD | Leitura completa, volume×qualidade, pendência G-001 e regressão F1/F2. | Bateria humana + comparação fonte×dashboard. | F3-T03 aceita. | Parar se esconder lacuna ou misturar volume com qualidade. | BLOQUEADA (F3-T03) |
| F3-T05 | 3 | Materializar registro de handoff com SLA, escalonamento e recuperação com massa sintética. | Maestro → Skip "repositório adapta" | Lista "Passagens pendentes": para quem a demanda foi passada, prazo para aceitar e se foi aceita, recusada ou venceu. | João Paulo | F3-003 | CA-3-06 | Dados e integrações; Fluxo e regras; Checklist | HO-F3-001 com destinatário, prazo de aceite, aceite, recusa e expiração. | Captura/export do registro e estados. | F3-T01 aceita (atingido 04/11); papéis provisórios registrados. | Parar se exigir disparo externo ou encerramento sem responsável. | LIBERADA (F3-T01 concluída em 04/11) |
| F3-T06 | 4 | Provar handoff com SLA, escalonamento e recuperação sem disparo externo. | Maestro → Skip "repositório adapta" | O registro de passagem aceito, recusado e vencido, com o prazo cumprido ou escalado. | João Paulo | F3-003 | CA-3-06/07 | Critérios de aceite; TDD | RED/GREEN/regressão; prova de expiração e escalonamento. | Bateria humana + log | F3-T05 aceita. | Parar se depender de e-mail real para operar. | BLOQUEADA (F3-T05) |
| F3-T07 | 3 | Criar o objeto inativo com prova negativa de leitura comercial. | Maestro → Skip "repositório adapta" | O registro arquivado recusando acesso comercial, com a prova de que não aparece para quem não deve ver. | João Paulo | F3-004 | CA-3-09 | Dados e integrações; Checklist | Objeto inativo + permissões server-side + prova negativa. | Captura/export da recusa | F3-T01 aceita (atingido 04/11); B3-DHO-01 para dado real. | Parar se exigir dado real ou se a recusa não for server-side. | LIBERADA (fixture sintética) |
| F3-T08 | 4 | Provar a recusa de leitura comercial e a decisão C-06 registrada. | Maestro → Skip "repositório adapta" | A tentativa de leitura comercial recusada, com a decisão registrada e datada. | João Paulo | F3-004 | CA-3-09/10 | Critérios de aceite; TDD | RED/GREEN/regressão; prova negativa repetida. | Bateria humana + decisão C-06 | F3-T07 aceita. | Parar se a recusa depender do frontend. | BLOQUEADA (F3-T07) |

## Automações da fase

| Automação | Task | Fonte | Destino | Nunca faz | Aviso | Estado |
|---|---|---|---|---|---|---|
| Campanhas Meta → "repositório adapta" | F3-T12/F3-T13 | métricas de campanha da conta `act_1667348577717128` (Graph API v21.0 direta, token em `META_ACCESS_TOKEN`) | `campanhas_meta`, `conexao_meta` | escrever na Meta; ler dado pessoal; gravar em outro projeto | faixa na tela + e-mail ao Champion | **Ativa e estável** — 70 linhas por execução entre 31/10 e 04/11, zero erro; cron a cada 2h + rede de segurança no ETHOS |

## Bloqueios transversais

- **B3-META-01 (alterado 22/10):** a F3 só lê métricas de campanha Meta pela automação da SPEC-F3-006; sem dado pessoal e sem escrita na Meta.
