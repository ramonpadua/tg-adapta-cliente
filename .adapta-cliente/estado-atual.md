# Estado atual — Adapta Cliente

- task_id: nenhuma
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-005-funil-kanban.md
- fase: 3
- task_elegivel: F3-T11 (SUSPENSA até concluir a correção de segurança; exige autorização do Champion)
- tasks_fase_3: 5/13 concluídas (F3-T09, F3-T12, F3-T01, F3-T13, F3-T10)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_teste_humano
- autorizacao_implementacao: SEGURANÇA autorizada em 11/11 12:51 (caminho B), 6 etapas, uma rota/tela por vez com homologação. ETAPA 1 HOMOLOGADA (16:21). ETAPA 2 HOMOLOGADA (17:24). ETAPA 3: /funil HOMOLOGADA (19:04); /pipeline-t01 entregue (19:12). Inicial mantém autenticação própria por ordem do owner. F3-T11 suspensa. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12); 2026-10-28 21:31 (agendador); 2026-10-30 (Graph API direta); 2026-11-04 14:30 (testes F3-T13); 2026-11-04 17:35 (retomar T10/T11).
- teste_humano: F3-T10 APROVADA. SEGURANÇA ETAPA 1 HOMOLOGADA (16:21); ETAPA 2 HOMOLOGADA (17:24); ETAPA 3 rota /funil HOMOLOGADA (19:04, owner confirmou os 13 cartões após Entrar); rota /pipeline-t01 ENTREGUE (19:12), aguardando homologação.
- verificacao_automatica: SEGURANÇA etapas 1 e 2 HOMOLOGADAS. Etapa 3 em duas rotas: /funil (v0.0.168) e /pipeline-t01 (v0.0.169), QA OK em ambas. Testes da /pipeline-t01: sem login root 20.588 B com porta de entrada e sem PIPE-F3-IN-001; após Entrar root 42.990 B com PIPE-F3-IN-001 e "Pipeline (T01)". Funil 13 de 13 (root 100.046 B); Inicial root 61.481 B com painel. Proteções: escrita anônima em demandas 404, criar conta 403. BASELINE intacto: demandas 13, propostas 2, vagas 2, pipeline_log 9, campanhas_meta 200, experimentos_f2 3, decisoes_f2 6, aprovacoes_f2 4.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-11 19:12 — SEGURANÇA ETAPA 3 (2ª rota) na v0.0.169: `ProtecaoDeRota` também embrulha /pipeline-t01, por FORA. Testes: sem login pede acesso e não mostra dados; com login mostra PIPE-F3-IN-001; Funil 13 de 13; Inicial com painel; proteções ativas; dados intactos. PENDÊNCIA CRÍTICA registrada antes: credenciais expostas no código público (6 telas) — bloqueia uso real até a Etapa 4.
- proxima_acao: aguardar homologação da ETAPA 3 na rota /pipeline-t01 (v0.0.169). Aprovada, embrulhar a próxima rota. A Inicial mantém autenticação própria.
- atualizado_em: 2026-11-11T19:12:00-03:00

## Defaults conservadores da F3-T01 (protocolo anti-trava — Constituição §Dúvidas e insumos, 07/10/2026)

- **D1 (vocabulário R&S/Alocação/SOS):** fixtures usam R&S / TMO / R&S + TMO como `oferta_servico`.
- **D2 (área + lista v1):** 13 áreas; "Não informado" = condição provisória (DECISÃO 08/10).
- **D3 (RN-F3-002 × Não informado):** não bloqueia avanço; confirmação humana (DECISÃO 08/10).
- **D4 (estado do retorno):** EMENDADO (E-F3-001-04) — vaga + log append-only; E2 liberada.
- **D5 (encerramento da substituição):** motivo obrigatório; não encerra a vaga; E5 liberada.
- **D6 (recuperação/RN-F3-004):** 3 tentativas, 90 dias, confirmação humana, nada automático; E6 liberada.

## DÚVIDAs documentais pendentes

NENHUMA — D1–D6 resolvidas pela emenda E-F3-001-04 (26/10/2026), conferida 15/15 em 28/10/2026.

## Ciclo da F3-T13 — concluída em 04/11/2026 (v0.0.146)

- **Credencial:** token do segredo `META_ACCESS_TOKEN` e `credentials/meta_token.json` são o MESMO (sha256 `7080ebf8c792`), principal João Paulo Oliveira.
- **Quatro defeitos reais corrigidos:** ensaio sujava o estado (v0.0.140); hora crua em UTC (v0.0.146); token expirado volta 400/190 e não 401 (v0.0.145); textos prometiam o que não acontecia (v0.0.143).
- **Homologação**: "Aprovado — pode concluir a T13" (04/11 14:53).

## F3-T10 — CONCLUÍDA (11/11/2026)

**Cinco etapas, cinco homologações:** E1 colunas e contagens → E2 cartão completo → E3 filtros → E4 Jornada → E5 arrastar com evidência.

**A prova do CA-3-14 é do próprio owner.** Ele arrastou "Serviços Delta Ltda." (`PIPE-F3-IN-001`) de Oportunidade para Proposta, com evidência. Evento `oportunidade → proposta` às 15:11:30 de 11/11.

**Quatro defeitos reais corrigidos:** zero falso sem sessão; contador fora do filtro; hora crua em UTC; tela em branco por ordem de hooks.

**Nenhuma coleção, campo, índice ou migration de estrutura nesta task.**

**Ocorrências preservadas:** eventos de teste no histórico em `06_notas/registro-eventos-teste-e5-2026-11-05.md`.

## Análise da F3-T10/T11

- **O código do app NÃO está no GitHub** — vive no projeto Skip 51414 (hoje v0.0.169).
- **O endereço de PREVIEW é o que funciona**: `repositorio-adapta-cc556--preview.goskip.app`. O de produção nunca foi publicado.
- **B3-RACI-01 NÃO bloqueia a T10/T11** — a matriz é exigida na F3-T02 e na F3-T05/T06.
- **Decisão registrada**: `campanha` é texto livre, sem vínculo com `campanhas_meta`.

## SEGURANÇA — correção em 6 etapas (autorizada 11/11/2026 12:51)

**Ordem:** (1) bloquear escrita anônima em demandas preservando leitura ✅ → (2) bloquear cadastro público e autoatribuição de papel ✅ → (3) login por rota → (4) conta administrativa real → (5) fechar leitura pública → (6) bloquear exclusão pública em `decisoes_f2`.

**Backups:** `work/seguranca/backup-regras-ANTES.json`, `backup-regras-ETAPA2.json`, `backup-v0167/`.

### Etapa 1 — APLICADA (v0.0.159) e HOMOLOGADA (16:21)

`demandas`: createRule e updateRule → `@request.auth.id != ''`; deleteRule → `null`; listRule/viewRule preservados.

### Etapa 2 — APLICADA (v0.0.160) e HOMOLOGADA (17:24)

`users`: createRule → `null` (ninguém cria conta pela API); updateRule → `id = @request.auth.id && @request.body.role:isset = false`; list/view/delete preservados.

### Etapa 3 — EM ANDAMENTO (uma rota por vez)

**Método:** o controle fica **fora** da página, embrulhando a rota no `App.tsx`. Nenhuma linha de `FunilF3Page.tsx`, `Index.tsx` ou `PipelineF3T01Page.tsx` foi alterada.

| Rota | Versão | Estado |
|---|---|---|
| `/funil` | v0.0.168 | **HOMOLOGADA** pelo owner em 19:04 |
| `/pipeline-t01` | v0.0.169 | ENTREGUE, aguardando homologação |
| `/` (Inicial) | — | mantém autenticação própria (`useAcessoRequerido`) |

Componente novo: `src/components/seg/ProtecaoDeRota.tsx`. Único arquivo existente alterado: `src/App.tsx`.

**Testes da /pipeline-t01:** sem login → porta de entrada, sem `PIPE-F3-IN-001` (root 20.588 B); após Entrar → `PIPE-F3-IN-001` e "Pipeline (T01)" (root 42.990 B). Funil 13 de 13; Inicial com painel; proteções ativas; baseline intacto.

## Incidente do Funil em branco (v0.0.161 a v0.0.166)

Ao aplicar o login **dentro** do Funil, o Maestro apagou três partes do componente ao desfazer. Tela em branco por 6 versões. Corrigido na v0.0.167 e confirmado pelo owner. **Lição adotada:** proteger por fora da página, nunca mexer no corpo de componente homologado. Relatório: `artifacts/INCIDENTE-Funil-em-branco-2026-11-11.md`.

## PENDÊNCIA CRÍTICA BLOQUEANTE — credenciais expostas

**ABERTA. Bloqueia o uso real do sistema.** Registrada em 11/11/2026 19:06 por decisão do owner.

A senha do usuário de teste está em texto aberto no código público: **6 cópias, em 6 telas** (Campanhas, Pipeline, Prova Meta, Experimentos F2, AcessoRequerido, ProtecaoDeRota). A credencial tem papel `champion`.

**Antes de liberar para uso real, é obrigatório:** (1) remover as senhas do código; (2) revogar/desativar a credencial antiga; (3) validar os acessos. **Até lá: somente dados fictícios.**

Gatilho: Etapa 4. A Etapa 5 não faz sentido antes disso. Documento: `06_notas/PENDENCIA-CRITICA-credenciais-expostas.md`.

## Pendências registradas

- **(a) Auditoria:** o histórico grava o `responsavel` da demanda, não quem executou.
- **(b) Contas reais:** criar para Comercial, Operação e Diretoria (etapa 4), com nomes indicados pelo owner.
- **(c) Rotas de backend:** varrer todas antes da etapa 5.
- **(d) Funil sem limite de espera:** se o Skip travar, a tela fica em "Carregando".
- **(e) Credenciais no código:** ver PENDÊNCIA CRÍTICA acima.

**Etapas 4 a 6:** não iniciadas.
