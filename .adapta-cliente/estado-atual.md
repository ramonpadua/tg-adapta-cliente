# Estado atual — Adapta Cliente

- task_id: nenhuma
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-005-funil-kanban.md
- fase: 3
- task_elegivel: F3-T11 (SUSPENSA até concluir a correção de segurança; exige autorização do Champion)
- tasks_fase_3: 5/13 concluídas (F3-T09, F3-T12, F3-T01, F3-T13, F3-T10)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: bloqueada
- autorizacao_implementacao: SEGURANÇA autorizada pelo owner em 11/11 12:51 (caminho B), em 6 etapas ordenadas, uma por vez com homologação entre elas. F3-T11 SUSPENSA até concluir. Etapa 1 (demandas: bloquear escrita, preservar leitura) APLICADA e entregue para homologação. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12); 2026-10-28 21:31 (agendador); 2026-10-30 (Graph API direta); 2026-11-04 14:30 (testes F3-T13); 2026-11-04 17:35 (retomar T10/T11).
- teste_humano: F3-T10 APROVADA nas cinco etapas. A prova final do CA-3-14 é do owner: ele moveu PIPE-F3-IN-001 (Serviços Delta) de Oportunidade para Proposta com evidência, e o sistema gravou a transição no histórico às 11/11 15:11:30, com as colunas acompanhando. F3-T13 homologada 04/11 14:53; F3-T12 em 03/11. SEGURANÇA etapa 1 ENTREGUE em 11/11 13:05, aguardando homologação do owner.
- verificacao_automatica: F3-T10 PASSOU nas 5 etapas (E1 v0.0.147, E2 v0.0.150, E3 v0.0.152, E4 v0.0.153, E5 v0.0.158). SEGURANÇA etapa 1 APLICADA em v0.0.159 (ref 086ad37), QA setup/static/build/integrations OK. Testes: T1 leitura sem login 13 registros PASSOU; T2 alterar sem login RECUSADO (404); T3 apagar sem login RECUSADO (403 Only superusers); T4 criar sem login RECUSADO (400). Telas conferidas sem sessão: Funil 13 de 13, Inicial com painel e modo homologação, sem erro. BASELINE PRESERVADO: demandas 13, propostas 2, vagas 2, pipeline_log 9, campanhas_meta 200, conexao_meta 205, experimentos_f2 3, decisoes_f2 6, aprovacoes_f2 4 — idêntico. Estados 9/1/2/1 inalterados.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-11 13:05 — SEGURANÇA ETAPA 1 de 6 aplicada e entregue (v0.0.159): `demandas` deixa de aceitar criar/alterar/apagar sem login; leitura preservada de propósito até a etapa 5. Backup das regras originais gravado antes da mudança.
- proxima_acao: aguardar homologação da ETAPA 1 de segurança; só então aplicar a etapa 2 (bloqueio do cadastro público de usuários)
- atualizado_em: 2026-11-11T13:05:00-03:00

## Defaults conservadores da F3-T01 (protocolo anti-trava — Constituição §Dúvidas e insumos, 07/10/2026)

Adotados pelo Champion em 09/10/2026 enquanto as emendas não chegam; nenhuma DÚVIDA é considerada resolvida:

- **D1 (vocabulário R&S/Alocação/SOS):** fixtures usam exclusivamente R&S / TMO / R&S + TMO como valores de `oferta_servico` (decisão Champion 07/10, changelog); "R&S/Alocação/SOS" na SPEC é lido como descrição da operação que devolve o retorno, não valor de campo. SOS = Serviço Orientado a Substituições, dentro do TMO.
- **D2 (área + lista v1):** área = departamento funcional do cargo; lista fechada v1 com 13 áreas (TI, Comercial/Vendas, Logística/Supply Chain, Projetos/PMO, Engenharia/Manutenção, Financeiro/Contábil, Administrativo, Marketing, Recursos Humanos, Operações/Produção, Educacional/Acadêmico, Auditoria/Riscos, Laboratorial/Saúde); "Não informado" = condição provisória, não área definitiva (DECISÃO 08/10).
- **D3 (RN-F3-002 × Não informado):** área "Não informado" não bloqueia avanço e não gera pendência automática nova; confirmação humana responsável (DECISÃO 08/10).
- **D4 (persistência do estado do retorno):** EMENDADO (E-F3-001-04, 26/10) — estado corrente na própria vaga + cada alteração também no log append-only; E2 liberada.
- **D5 (encerramento da substituição):** EMENDADO (E-F3-001-04, 26/10) — "Substituição encerrada" aprovado com motivo obrigatório ("reposto" ou "encerrada sem reposição"); não encerra a vaga automaticamente; E5 liberada.
- **D6 (regras de recuperação/RN-F3-004):** EMENDADO (E-F3-001-04, 26/10) — regras de 24/09 incorporadas ao corpo da SPEC (3 tentativas, 90 dias corridos, sugestão de "sem timing" com confirmação expressa do Comercial, nada automático); E6 liberada.

## DÚVIDAs documentais pendentes (lote único ao consultor)

NENHUMA — D1–D6 resolvidas pela emenda E-F3-001-04 (26/10/2026), conferida 15/15 em 28/10/2026.


## Consolidação documental publicada em 29/10/2026

SPEC F3-001, fase.md, Tasks Gerais, matriz e fixtures foram alinhados com E-F3-001-04. D4/D5/D6 não são bloqueios documentais vigentes.

Histórico técnico de 28/10 e aceites anteriores preservados integralmente. Uma task por vez; cada uma exige autorização do Champion antes de implementar. E4 continua plano sem emenda/task liberada; dados pessoais reais continuam condicionados à política LGPD aprovada.

## Ciclo da F3-T13 — concluída em 04/11/2026 (v0.0.146)

- **Pedido do owner**: "esse token até já criou campanhas, excluiu campanhas inativas. é só ele que deve ficar aqui ok? agora refaça os testes da tarefa usando esse token oficial".
- **Credencial conferida**: o token do segredo `META_ACCESS_TOKEN` e o arquivo `credentials/meta_token.json` são o MESMO (impressão digital sha256 `7080ebf8c792` nos dois), principal João Paulo Oliveira, sem expiração.
- **Correção de afirmação errada**: a rota `f3-t13/token-info` dizia `escrita: false` ao lado do token. Agora separa `token_pode_escrever_na_meta: true` de `automacao_escreve_na_meta: false`.
- **DEFEITO REAL 1 — o ensaio sujava o estado (v0.0.140)**: ensaio chamava `registrarSaude()` e `avisarChampion()`. Corrigido: ensaio não grava saúde nem envia e-mail. Resíduo removido pela migration 0035.
- **DEFEITO REAL 2 — hora crua em UTC na faixa (v0.0.146)**: criada `formatarInstanteBR()` com 4 casos testados.
- **DEFEITO REAL 3 — token expirado volta HTTP 400/190, não 401 (v0.0.145)**: comprovado contra a Graph API com três tokens inválidos.
- **DEFEITO REAL 4 — textos prometiam o que não acontecia (v0.0.143)**: alinhados ao comportamento real.
- **Verificação independente**: prova de falha real com credencial inválida → e-mail no log, faixa na tela, 130 linhas preservadas; reexecução 0 criadas / 70 atualizadas.
- **Homologação**: "Aprovado — pode concluir a T13" (04/11/2026 14:53).

## Análise da F3-T10/T11 (04/11/2026)

- **Autorização do owner (04/11 17:35)**: "Autorizo retomar exclusivamente a F3-T10/T11"; "avançar sem aguardar a matriz B3-RACI-01"; "Execute em etapas testáveis"; "Não conclua T10/T11 sem os testes previstos e minha homologação".
- **B3-RACI-01 NÃO bloqueia a T10/T11** — a matriz é exigida na F3-T02 e na F3-T05/T06.
- **O código do app NÃO está no GitHub** — vive no projeto Skip 51414, versionado por apply (hoje v0.0.159).
- **Reaproveitamento total — sem migration de estrutura**: `demandas`, `propostas`, `vagas`, `pipeline_log`.
- **Decisão registrada**: `campanha` é texto livre, sem vínculo com `campanhas_meta`. Não foi inventada ligação.

## F3-T10 — CONCLUÍDA (11/11/2026)

**Cinco etapas, cinco homologações:** E1 colunas e contagens → E2 cartão completo → E3 filtros → E4 Jornada → E5 arrastar com evidência.

**A prova do CA-3-14 é do próprio owner.** Ele arrastou "Serviços Delta Ltda." (`PIPE-F3-IN-001`) de Oportunidade para Proposta, com evidência. Conferido: `estado = proposta`, evento `oportunidade → proposta` às 15:11:30 de 11/11, colunas acompanhando (Oportunidade 3→2, Proposta 0→1).

**Quatro defeitos reais corrigidos:** (1) zero falso sem sessão; (2) contador fora do filtro; (3) hora crua em UTC; (4) tela em branco por ordem de hooks.

**Limites declarados na tela:** `campanha` é texto livre; o filtro "incluir dados de teste" começa marcado.

**Nenhuma coleção, campo, índice ou migration de estrutura nesta task.**

**Ocorrências preservadas:** eventos de teste no histórico em `06_notas/registro-eventos-teste-e5-2026-11-05.md`.

## SEGURANÇA — correção em 6 etapas (autorizada 11/11/2026 12:51)

**Ordem determinada pelo owner:** (1) bloquear escrita anônima em demandas preservando leitura → (2) bloquear cadastro público de usuários e impedir autoatribuição de `champion`/`delegado` → (3) acrescentar login nas telas Inicial, Funil e Pipeline usando o sistema de autenticação existente → (4) preparar conta administrativa real (sem criar conta sem indicação do titular) → (5) fechar leitura pública de demandas → (6) bloquear exclusão pública em `decisoes_f2`.

**Backup do estado original:** `work/seguranca/backup-regras-ANTES.json` guarda as regras exatas de `demandas`, `users` e `decisoes_f2` antes de qualquer mudança. Cada migration tem função de reversão.

### Etapa 1 — APLICADA (v0.0.159, ref 086ad37)

| Regra (demandas) | Antes | Depois |
|---|---|---|
| `createRule` | `""` (livre) | `@request.auth.id != ''` |
| `updateRule` | `""` (livre) | `@request.auth.id != ''` |
| `deleteRule` | `""` (livre) | `null` (ninguém apaga pela API) |
| `listRule` | `""` | **preservado** `""` |
| `viewRule` | `""` | **preservado** `""` |

**Testes (nenhum gravou dado comercial):** T1 leitura sem login → 13 registros PASSOU; T2 alterar sem login → RECUSADO (404); T3 apagar sem login → RECUSADO (403); T4 criar sem login → RECUSADO (400). Telas sem sessão: Funil 13 de 13; Inicial com painel; sem erro.

**Baseline preservado:** demandas 13, propostas 2, vagas 2, pipeline_log 9, campanhas_meta 200, conexao_meta 205, experimentos_f2 3, decisoes_f2 6, aprovacoes_f2 4. Estados 9/1/2/1 inalterados.

**Erro do Maestro no teste, confessado e corrigido:** ao repetir T2 logado, enviou `proxima_acao: "x"` e o servidor aceitou. Restaurado para `"Retomar contato — sem resposta do cliente"`, conferido campo a campo. O histórico não mudou (9 eventos): alterar `proxima_acao` não gera evento.

**Pendências registradas:** (a) auditoria — o histórico grava o `responsavel` da demanda, não quem executou; (b) criar contas reais para Comercial, Operação e Diretoria; (c) verificar outros caminhos de acesso público aos mesmos dados (rotas de backend).

**Etapas 2 a 6:** não iniciadas.
