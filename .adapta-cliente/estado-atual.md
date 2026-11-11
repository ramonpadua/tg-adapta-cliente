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
- autorizacao_implementacao: SEGURANÇA autorizada em 11/11 12:51 (caminho B), 6 etapas. ETAPA 1 HOMOLOGADA (16:21). ETAPA 2 HOMOLOGADA (17:24). ETAPA 3 PARCIALMENTE autorizada (18:49): aplicar `ProtecaoDeRota` SOMENTE na rota /funil, sem tocar no código interno de Funil, Inicial ou Pipeline, e mantendo a Inicial como está. F3-T11 suspensa. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12); 2026-10-28 21:31 (agendador); 2026-10-30 (Graph API direta); 2026-11-04 14:30 (testes F3-T13); 2026-11-04 17:35 (retomar T10/T11).
- teste_humano: F3-T10 APROVADA (5 etapas). SEGURANÇA ETAPA 1 HOMOLOGADA em 11/11 16:21. SEGURANÇA ETAPA 2 HOMOLOGADA em 11/11 17:24. ETAPA 3 (parcial, só /funil) ENTREGUE em 11/11 18:55, aguardando homologação. A Inicial tem autenticação própria e foi mantida como está por ordem do owner.
- verificacao_automatica: SEGURANÇA etapa 1 HOMOLOGADA (v0.0.159); etapa 2 HOMOLOGADA (v0.0.160); etapa 3 PARCIAL aplicada em v0.0.168 (ref 3d9582a), QA OK. Testes da etapa 3: (1) sem login — /funil root 20.571 B, mostra a porta de entrada, 0 cartões; (2) clique em Entrar — root 100.046 B, "mostrando 13 de 13", 13 cartões; (3) Inicial root 61.481 B com painel; Pipeline root 42.990 B com PIPE-F3-IN-001; Campanhas root 57.554 B com 10 campanhas e "Conexão com a Meta ativa"; (4) proteções ativas: escrita anônima em demandas 404, apagar 403, criar conta 403, role no pedido 404. BASELINE intacto: demandas 13, propostas 2, vagas 2, pipeline_log 9, campanhas_meta 200, experimentos_f2 3, decisoes_f2 6, aprovacoes_f2 4, contas 1.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-11 18:55 — SEGURANÇA ETAPA 3 (parcial) aplicada na v0.0.168: componente `ProtecaoDeRota` embrulha APENAS a rota /funil, por FORA — a página FunilF3Page não foi tocada. Testes: sem login pede autenticação e não mostra cartões; com login mostra 13 de 13; Inicial, Pipeline e Campanhas funcionando; proteções das etapas 1 e 2 ativas; dados intactos.
- proxima_acao: aguardar homologação da ETAPA 3 (só a rota /funil). Aprovada, embrulhar as outras rotas conforme ordem do owner. Inicial mantida como está.
- atualizado_em: 2026-11-11T18:55:00-03:00

## Defaults conservadores da F3-T01 (protocolo anti-trava — Constituição §Dúvidas e insumos, 07/10/2026)

Adotados pelo Champion em 09/10/2026 enquanto as emendas não chegam; nenhuma DÚVIDA é considerada resolvida:

- **D1 (vocabulário R&S/Alocação/SOS):** fixtures usam exclusivamente R&S / TMO / R&S + TMO como valores de `oferta_servico`; "R&S/Alocação/SOS" na SPEC é lido como descrição da operação que devolve o retorno, não valor de campo. SOS = Serviço Orientado a Substituições, dentro do TMO.
- **D2 (área + lista v1):** área = departamento funcional do cargo; lista fechada v1 com 13 áreas; "Não informado" = condição provisória, não área definitiva (DECISÃO 08/10).
- **D3 (RN-F3-002 × Não informado):** área "Não informado" não bloqueia avanço e não gera pendência automática nova; confirmação humana responsável (DECISÃO 08/10).
- **D4 (persistência do estado do retorno):** EMENDADO (E-F3-001-04, 26/10) — estado corrente na própria vaga + cada alteração também no log append-only; E2 liberada.
- **D5 (encerramento da substituição):** EMENDADO (E-F3-001-04, 26/10) — "Substituição encerrada" aprovado com motivo obrigatório ("reposto" ou "encerrada sem reposição"); não encerra a vaga automaticamente; E5 liberada.
- **D6 (regras de recuperação/RN-F3-004):** EMENDADO (E-F3-001-04, 26/10) — 3 tentativas, 90 dias corridos, sugestão de "sem timing" com confirmação expressa do Comercial, nada automático; E6 liberada.

## DÚVIDAs documentais pendentes (lote único ao consultor)

NENHUMA — D1–D6 resolvidas pela emenda E-F3-001-04 (26/10/2026), conferida 15/15 em 28/10/2026.

## Ciclo da F3-T13 — concluída em 04/11/2026 (v0.0.146)

- **Credencial:** token do segredo `META_ACCESS_TOKEN` e `credentials/meta_token.json` são o MESMO (sha256 `7080ebf8c792`), principal João Paulo Oliveira, sem expiração.
- **DEFEITO REAL 1 — o ensaio sujava o estado (v0.0.140)**: corrigido; resíduo removido pela migration 0035.
- **DEFEITO REAL 2 — hora crua em UTC na faixa (v0.0.146)**: criada `formatarInstanteBR()`.
- **DEFEITO REAL 3 — token expirado volta HTTP 400/190, não 401 (v0.0.145)**: comprovado contra a Graph API.
- **DEFEITO REAL 4 — textos prometiam o que não acontecia (v0.0.143)**: alinhados.
- **Homologação**: "Aprovado — pode concluir a T13" (04/11/2026 14:53).

## F3-T10 — CONCLUÍDA (11/11/2026)

**Cinco etapas, cinco homologações:** E1 colunas e contagens → E2 cartão completo → E3 filtros → E4 Jornada → E5 arrastar com evidência.

**A prova do CA-3-14 é do próprio owner.** Ele arrastou "Serviços Delta Ltda." (`PIPE-F3-IN-001`) de Oportunidade para Proposta, com evidência. Conferido: `estado = proposta`, evento `oportunidade → proposta` às 15:11:30 de 11/11.

**Quatro defeitos reais corrigidos:** (1) zero falso sem sessão; (2) contador fora do filtro; (3) hora crua em UTC; (4) tela em branco por ordem de hooks.

**Nenhuma coleção, campo, índice ou migration de estrutura nesta task.**

**Ocorrências preservadas:** eventos de teste no histórico em `06_notas/registro-eventos-teste-e5-2026-11-05.md`.

## Análise da F3-T10/T11

- **O código do app NÃO está no GitHub** — vive no projeto Skip 51414 (hoje v0.0.168).
- **O endereço de PREVIEW é o que funciona**: `repositorio-adapta-cc556--preview.goskip.app`. O de produção nunca foi publicado.
- **B3-RACI-01 NÃO bloqueia a T10/T11** — a matriz é exigida na F3-T02 e na F3-T05/T06.
- **Decisão registrada**: `campanha` é texto livre, sem vínculo com `campanhas_meta`. Não foi inventada ligação.

## SEGURANÇA — correção em 6 etapas (autorizada 11/11/2026 12:51)

**Ordem determinada pelo owner:** (1) bloquear escrita anônima em demandas preservando leitura ✅ → (2) bloquear cadastro público e impedir autoatribuição de papel ✅ → (3) login nas telas Inicial, Funil e Pipeline — **parcial: só /funil** → (4) conta administrativa real → (5) fechar leitura pública de demandas → (6) bloquear exclusão pública em `decisoes_f2`.

**Backups:** `work/seguranca/backup-regras-ANTES.json`, `backup-regras-ETAPA2.json` e `backup-v0167/` (App.tsx, FunilF3Page, bundle, LEIA-ME). Cada migration tem função de reversão.

### Etapa 1 — APLICADA (v0.0.159) e HOMOLOGADA (11/11 16:21)

| Regra (demandas) | Antes | Depois |
|---|---|---|
| `createRule` | `""` | `@request.auth.id != ''` |
| `updateRule` | `""` | `@request.auth.id != ''` |
| `deleteRule` | `""` | `null` |
| `listRule` / `viewRule` | `""` | **preservados** |

### Etapa 2 — APLICADA (v0.0.160) e HOMOLOGADA (11/11 17:24)

| Regra (users) | Antes | Depois |
|---|---|---|
| `createRule` | `""` (livre) | `null` — ninguém cria conta pela API |
| `updateRule` | `id = @request.auth.id` | `id = @request.auth.id && @request.body.role:isset = false` |
| `listRule` / `viewRule` / `deleteRule` | `id = @request.auth.id` | **preservados** |

### Etapa 3 (parcial) — APLICADA (v0.0.168, ref 3d9582a), aguardando homologação

**Método novo, depois do incidente:** o controle fica **fora** da página, embrulhando a rota no `App.tsx`. Nenhuma linha de `FunilF3Page.tsx`, `Index.tsx` ou `PipelineF3T01Page.tsx` foi alterada.

- Componente novo: `src/components/seg/ProtecaoDeRota.tsx` (150 linhas, isolado).
- Único arquivo existente alterado: `src/App.tsx` — 1 import + 1 rota embrulhada.
- A Inicial **manteve** sua autenticação própria (`useAcessoRequerido`), por ordem do owner.

**Testes:** sem login → porta de entrada, 0 cartões (root 20.571 B); com login → 13 de 13 cartões (root 100.046 B); Inicial, Pipeline e Campanhas funcionando; proteções das etapas 1 e 2 ativas; baseline intacto.

## Incidente do Funil em branco (11/11/2026, v0.0.161 a v0.0.166)

Ao tentar aplicar o login **dentro** do Funil, o Maestro apagou três partes do componente ao desfazer (`carregar`, sua dependência e a variável `ativos`). A tela ficou em branco por 6 versões. Corrigido na v0.0.167 e confirmado pelo owner. **Lição adotada:** proteger por fora da página, nunca mexer no corpo de componente homologado. Relatório: `artifacts/INCIDENTE-Funil-em-branco-2026-11-11.md`.

## Pendências registradas

- **(a) Auditoria:** o histórico grava o `responsavel` da demanda, não quem executou a alteração.
- **(b) Contas reais:** criar para Comercial, Operação e Diretoria (etapa 4), com nomes indicados pelo owner.
- **(c) Rotas de backend:** varrer todas antes da etapa 5.
- **(d) Funil sem limite de espera:** se o Skip travar, a tela fica em "Carregando". Sugerido tratar depois das 6 etapas.
- **(e) Credenciais no código:** `ProtecaoDeRota` e as telas homologadas usam o usuário de validação. Só para homologação — sai quando houver contas reais (etapa 4).

**Etapas 4 a 6:** não iniciadas.
