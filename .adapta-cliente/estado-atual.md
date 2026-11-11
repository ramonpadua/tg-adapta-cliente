# Estado atual — Adapta Cliente

- task_id: nenhuma
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-005-funil-kanban.md
- fase: 3
- task_elegivel: F3-T11 (próxima; as provas do Funil — exige autorização do Champion)
- tasks_fase_3: 5/13 concluídas (F3-T09, F3-T12, F3-T01, F3-T13, F3-T10)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: sem_task
- autorizacao_implementacao: T10 ENCERRADA. Cinco etapas homologadas pelo owner: E1 (04/11 20:03), E2 (04/11 21:00), E3 (05/11 10:29), E4 (05/11 10:29) e E5 (11/11 12:10, "E5 aprovada"). O owner decidiu preservar os eventos de teste do histórico (05/11 11:02), não autoriza superusuário nem nova limpeza. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12); 2026-10-28 21:31 (agendador); 2026-10-30 (Graph API direta); 2026-11-04 14:30 (testes F3-T13); 2026-11-04 17:35 (retomar T10/T11).
- teste_humano: F3-T10 APROVADA nas cinco etapas. A prova final do CA-3-14 é do owner: ele moveu PIPE-F3-IN-001 (Serviços Delta) de Oportunidade para Proposta com evidência, e o sistema gravou a transição no histórico às 11/11 15:11:30, com as colunas acompanhando (Oportunidade 3→2, Proposta 0→1, soma 13). F3-T13 homologada 04/11 14:53; F3-T12 em 03/11. T11 pendente: não inicia sem autorização.
- verificacao_automatica: F3-T10 PASSOU nas 5 etapas (E1 v0.0.147, E2 v0.0.150, E3 v0.0.152, E4 v0.0.153, E5 v0.0.158), QA setup/static/build/integrations OK em todas. Verificação independente ao encerrar: contagem por coluna conferida contra a fonte (suspect 9, prospect 1, oportunidade 2, proposta 1, encerrados 0 — soma 13 = 13); histórico com o evento do arrasto do owner; regressão OK em /pipeline-t01 e /campanhas; nenhuma coleção criada. Quatro defeitos reais corrigidos: zero falso sem sessão, contador fora do filtro, hora crua em UTC e tela em branco por ordem de hooks. BASELINE: 13 demandas, 2 propostas, 2 vagas, campanhas_meta 200 (cresce pelo cron), experimentos_f2 3, decisoes_f2 6.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-11 12:12 — **F3-T10 CONCLUÍDA** (5/13 na Fase 3). Cinco etapas homologadas pelo owner; a prova final foi o arrasto dele em PIPE-F3-IN-001. Documentos atualizados: fase.md, STATUS.md, changelog em arquivo separado e este estado. Versão do app: v0.0.158.
- proxima_acao: apresentar a F3-T11 (as provas do Funil) e aguardar autorização do owner; a task não inicia sozinha
- atualizado_em: 2026-11-11T12:12:00-03:00

## Defaults conservadores da F3-T01 (protocolo anti-trava — Constituição §Dúvidas e insumos, 07/10/2026)

Adotados pelo Champion em 09/10/2026 enquanto as emendas não chegam; nenhuma DÚVIDA é considerada resolvida:

- **D1 (vocabulário R&S/Alocação/SOS):** fixtures usam exclusivamente R&S / TMO / R&S + TMO como valores de `oferta_servico` (decisão Champion 07/10, changelog); "R&S/Alocação/SOS" na SPEC é lido como descrição da operação que devolve o retorno, não valor de campo. SOS = Serviço Orientado a Substituições, dentro do TMO.
- **D2 (área + lista v1):** área = departamento funcional do cargo; lista fechada v1 com 13 áreas (TI, Comercial/Vendas, Logística/Supply Chain, Projetos/PMO, Engenharia/Manutenção, Financeiro/Contábil, Administrativo, Marketing, Recursos Humanos, Operações/Produção, Educacional/Acadêmico, Auditoria/Riscos, Laboratorial/Saúde); "Não informado" = condição provisória, não área definitiva (DECISÃO 08/10).
- **D3 (RN-F3-002 × Não informado):** área "Não informado" não bloqueia avanço e não gera pendência automática nova; confirmação humana responsável (DECISÃO 08/10).
- **D4 (persistência do estado do retorno):** EMENDADO (E-F3-001-04, 26/10) — estado corrente na própria vaga + cada alteração também no log append-only; E2 liberada.
- **D5 (encerramento da substituição):** EMENDADO (E-F3-001-04, 26/10) — "Substituição encerrada" aprovado com motivo obrigatório ("reposto" ou "encerrada sem reposição"); não encerra a vaga automaticamente; E5 liberada.
- **D6 (regras de recuperação/RN-F3-004):** EMENDADO (E-F3-001-04, 26/10) — regras de 24/09 incorporadas ao corpo da SPEC (3 tentativas, 90 dias corridos, sugestão de "sem timing" com confirmação expressa do Comercial, nada automático); E6 liberada.

## DÚVIDAs documentais pendentes (lote único ao consultor)

NENHUMA — D1–D6 resolvidas pela emenda E-F3-001-04 (26/10/2026), conferida 15/15 em 28/10/2026. Registros históricos das dúvidas permanecem no changelog (07–08/10 e 24–26/10) como registro do processo.


## Consolidação documental publicada em 29/10/2026

SPEC F3-001, fase.md, Tasks Gerais, matriz e fixtures foram alinhados com E-F3-001-04. D4/D5/D6 não são bloqueios documentais vigentes; não perguntar novamente onde persiste o retorno, quais motivos encerram substituição ou se a recuperação usa 3 tentativas/90 dias/confirmação humana.

Histórico técnico de 28/10 e aceites anteriores preservados integralmente. Uma task por vez; cada uma exige autorização do Champion antes de implementar. E4 continua plano sem emenda/task liberada; dados pessoais reais continuam condicionados à política LGPD aprovada.

## Ciclo da F3-T13 — concluída em 04/11/2026 (v0.0.146)

- **Pedido do owner**: "esse token até já criou campanhas, excluiu campanhas inativas. é só ele que deve ficar aqui ok? agora refaça os testes da tarefa usando esse token oficial".
- **Credencial conferida**: o token do segredo `META_ACCESS_TOKEN` e o arquivo `credentials/meta_token.json` são o MESMO (impressão digital sha256 `7080ebf8c792` nos dois), principal João Paulo Oliveira, sem expiração. Verificação por impressão digital, sem expor o valor.
- **Correção de afirmação errada**: a rota `f3-t13/token-info` dizia `escrita: false` ao lado do token, dando a entender que o token não escreve. O owner esclareceu que esse token já criou e excluiu campanhas. Agora a rota separa `token_pode_escrever_na_meta: true` (poder da credencial) de `automacao_escreve_na_meta: false` (comportamento do código).
- **DEFEITO REAL 1 — o ensaio sujava o estado (v0.0.140)**: o modo ensaio chamava `registrarSaude()` e `avisarChampion()`. Rodar "Rodar os cinco" gravou 6 linhas de 'erro' em `conexao_meta` e disparou e-mail de "Conexão Meta expirada" com a conexão saudável; a tela passou a mostrar "Saúde da conexão: erro". Corrigido: ensaio não grava saúde nem envia e-mail. Resíduo removido pela migration 0035 (apaga só linhas cuja mensagem começa com "Ensaio").
- **DEFEITO REAL 2 — hora crua em UTC na faixa (v0.0.146)**: a faixa mostrava "Expirou em 2026-11-04T17:57:03.485Z" onde em Brasília eram 14:57. Criada `formatarInstanteBR()` com 4 casos de teste (incluindo virada de dia), todos passaram.
- **DEFEITO REAL 3 — token expirado volta HTTP 400/190, não 401 (v0.0.145)**: comprovado contra a Graph API com três tokens inválidos. Uma expiração real cairia em "erro genérico" e a faixa mandaria procurar defeito onde faltava reconectar. Corrigido.
- **DEFEITO REAL 4 — textos prometiam o que não acontecia (v0.0.143)**: os cenários diziam "faixa de aviso e e-mail" e "erro registrado" depois que o ensaio deixou de fazer isso. Alinhados ao comportamento real.
- **Verificação independente (refeita do zero)**: prova de falha real com credencial inválida → Meta recusou, saúde `expirada`, e-mail enviado (confirmado no log do sistema às 17:57:03 UTC), faixa de aviso na tela, 130 linhas preservadas; reexecução com credencial boa: 0 criadas / 70 atualizadas / 0 duplicados / 0 escrita na Meta / destino único; regressão F1/F2/T01 intacta.
- **Homologação**: "Aprovado — pode concluir a T13" (04/11/2026 14:53).

## Análise da F3-T10/T11 (04/11/2026)

- **Autorização do owner (04/11 17:35)**: "Autorizo retomar exclusivamente a F3-T10/T11, referente à tela Funil"; "avançar sem aguardar a matriz B3-RACI-01, desde que ela não seja uma dependência obrigatória da T10/T11"; "Execute em etapas testáveis"; "Não conclua T10/T11 sem os testes previstos e minha homologação".
- **B3-RACI-01 NÃO bloqueia a T10/T11** — conferido na SPEC-F3-005: a matriz é exigida na F3-T02 (prova de equivalência) e na F3-T05/T06 (handoff). A T10/T11 não depende dela.
- **O código do app NÃO está no GitHub** (o repo `ramonpadua/tg-adapta-cliente` guarda só a documentação: 01_projeto…06_notas). O código vive no projeto Skip 51414, versionado por apply (hoje v0.0.158). Preservação se verifica contra o app, não contra o repo.
- **Reaproveitamento total — sem migration de estrutura**: `demandas` já tem empresa, estado, tipo_origem, canal, campanha, oferta_servico, responsavel, proxima_acao, prazo, origem_dado, evidencia; `propostas` (2) e `vagas` (2) ligam por `demanda_id`; `pipeline_log` tem estado_anterior, estado_novo, evidencia, quando, autor, demanda_id.
- **Faixas de estados conferidas**: `suspect`, `prospect`, `lead_qualificado`, `oportunidade`, `proposta`, `vaga_aberta`, terminais `ganho`/`perdido`/`sem_timing`/`desqualificado`.
- **Dado sintético**: 13 demandas, todas `origem_dado=sintetico`; 2 com `tipo_origem=desconhecido` (FIX-UNK-001 e ATR-F2-UNK-001); 6 sem `campanha` e 2 sem `canal`.
- **Decisão registrada**: `demandas` não tem campo `tipo` inbound/outbound — usa `tipo_origem`; e `campanha` é texto livre, sem vínculo com `campanhas_meta`. A Jornada mostra a campanha como texto. Não foi inventada ligação.
- **Verificação de integridade documental** (pedido do owner): troca pontual com conferência de contagens antes/depois (campos, seções `## `, D1–D6, histórico) e publicação por `push_files` quando o arquivo passa de ~16 KB, porque `create_or_update_file` trunca em silêncio nesse tamanho.

## F3-T10 — CONCLUÍDA (11/11/2026)

**Cinco etapas, cinco homologações:** E1 colunas e contagens → E2 cartão completo → E3 filtros → E4 Jornada → E5 arrastar com evidência. Cada uma foi entregue, testada pelo owner e aprovada antes da seguinte.

**A prova do CA-3-14 é do próprio owner.** Ele arrastou o cartão **"Serviços Delta Ltda."** (`PIPE-F3-IN-001`) de **Oportunidade** para **Proposta**, escrevendo a evidência. Resultado conferido na fonte: `estado = proposta`, evidência gravada, evento `oportunidade → proposta` no histórico às 15:11:30 de 11/11, e as colunas acompanhando (Oportunidade 3→2, Proposta 0→1, soma 13 = 13).

**A tela entregue:** cada demanda na sua etapa, com origem, canal, campanha, serviço, área, dono, próxima ação e prazo; filtros que recalculam os números junto com os cartões; painel de Jornada com a história completa; e arrastar que passa pela transição real do pipeline, com evidência obrigatória — sem mecanismo próprio.

**Quatro defeitos reais corrigidos:** (1) **zero falso sem sessão** — o banco devolve lista vazia com HTTP 200 em vez de erro, então o `try/catch` nunca disparava e a tela mostrava "0 propostas" como se fosse dado real; (2) **contador fora do filtro** — "Prazo vencido 9" ao lado de "Demandas 2", achado no print do owner; (3) **hora crua em UTC** na faixa; (4) **tela em branco** por usar uma função antes de declará-la — erro que o build do Skip não pega.

**Limites declarados na tela:** `campanha` é texto livre, sem vínculo com `campanhas_meta` (a tela diz isso ao lado do nome); e o filtro "incluir dados de teste" começa marcado porque 13 de 13 registros são sintéticos.

**Nenhuma coleção, campo, índice ou migration de estrutura nesta task.** O Funil é visão sobre dado existente (RN-F3-020). As migrations 0036/0037/0038 foram tentativas de limpeza de resíduo de teste e não tocaram schema.

**Ocorrências preservadas por decisão do owner:** eventos de teste técnico no histórico, registrados em `06_notas/registro-eventos-teste-e5-2026-11-05.md`, sem representar movimentação comercial.

**Próxima: F3-T11** — as provas do Funil (origem desconhecida, arrastar sem evidência recusado, contagens iguais à fonte e regressão do painel). Não inicia sem autorização do owner.
