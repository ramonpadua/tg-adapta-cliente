# Estado atual — Adapta Cliente

- task_id: F3-T10
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-005-funil-kanban.md
- fase: 3
- task_elegivel: F3-T10 (selecionada; F3-T11 vem depois dela, bloqueada até a T10 ser aceita)
- tasks_fase_3: 4/13 concluídas (F3-T09, F3-T12, F3-T01, F3-T13)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_teste_humano
- autorizacao_implementacao: CONFIRMADA para a T10 (04/11 17:41). E1, E2 e E3 APROVADAS. Em 05/11 11:02 o owner decidiu PRESERVAR os dois eventos de teste do histórico (não autoriza superusuário nem nova limpeza), pediu 5 verificações e AUTORIZOU continuar exclusivamente a interface de arrastar da E5, com dados sintéticos. Não concluir a E5 nem ir para a T11 sem aprovação. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12); 2026-10-28 21:31 (agendador); 2026-10-30 (Graph API direta); 2026-11-04 14:30 (testes F3-T13); 2026-11-04 17:35 (retomar T10/T11).
- teste_humano: E1 APROVADA (04/11 20:03); E2 APROVADA (04/11 21:00); E3 APROVADA (05/11 10:29); E5 — interface entregue em 05/11 11:20, aguardando homologação com o roteiro. F3-T13 homologada 04/11 14:53. T10/T11 pendentes de homologação final.
- verificacao_automatica: E1/E2/E3 PASSOU. E5 INTERFACE — Skip v0.0.158 (ref 3823250), QA setup/static/build/integrations OK. DEFEITO REAL corrigido na E5: a tela abria EM BRANCO (v0.0.157) porque `confirmarMover` usava `carregar` na lista de dependências antes de `carregar` ser declarado — erro de EXECUÇÃO, que o build do Skip não pega; corrigida a ordem dos hooks, 13 cartões arrastáveis conferidos no navegador. Teste de recusa SEM gerar evento: PATCH suspect->prospect em FIX-IN-001 devolve "Nao e possivel avancar para Prospect: handoff_status=aceito e obrigatorio", estado permanece suspect e pipeline_log segue em 4. As 5 verificações do resíduo estão no registro formal `06_notas/registro-eventos-teste-e5-2026-11-05.md`. BASELINE FINAL: demandas 13, propostas 2, vagas 2, pipeline_log 4 (2 legítimos + 2 do teste, preservados por decisão do owner), estados 9/1/3 idênticos, 0 registros de produção.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-05 11:20 — F3-T10 ETAPA E5 entregue (v0.0.158): arrastar cartão entre colunas abre o diálogo de evidência obrigatória; a transição é a da F1 (não um mecanismo novo); recusa mostra o motivo do servidor e o cartão não se move. Antes: 11:02 — 5 verificações do resíduo concluídas e registradas; owner decidiu preservar os 2 eventos.
- proxima_acao: aguardar homologação da E5 com o roteiro de arrastar; aprovada a E5, concluir a T10 e apresentar a T11 (as provas). Nada disso sem aprovação do owner
- atualizado_em: 2026-11-05T11:20:00-03:00

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
- **Reaproveitamento total — sem migration de estrutura**: `demandas` já tem empresa, estado, tipo_origem, canal, campanha, oferta_servico, responsavel, proxima_acao, prazo, origem_dado, evidencia; `propostas` (2) e `vagas` (2) ligam por `demanda_id`; `pipeline_log` (4) tem estado_anterior, estado_novo, evidencia, quando, autor, demanda_id.
- **Faixas de estados conferidas**: `suspect`, `prospect`, `lead_qualificado`, `oportunidade`, `proposta`, `vaga_aberta`, terminais `ganho`/`perdido`/`sem_timing`/`desqualificado`.
- **Dado sintético para a prova**: 13 demandas, todas `origem_dado=sintetico`; **2** com `tipo_origem=desconhecido` (FIX-UNK-001 e ATR-F2-UNK-001); outros 6 sem `campanha` e 2 sem `canal`.
- **Achado que exige decisão**: `demandas` não tem campo `tipo` inbound/outbound — usa `tipo_origem`; e `campanha` é texto livre, sem vínculo com `campanhas_meta`. A Jornada mostrará a campanha como texto, não como registro da F3-T12. Registrado como decisão, não inventado.
- **Baseline a preservar**: demandas=13, propostas=2, vagas=2, pipeline_log=4 (2 legítimos + 2 do teste preservados), campanhas_meta≈140 (sobe 10 a cada sync de 2h), conexao_meta≈114 (sobe 1 a cada 2h), experimentos_f2=3, decisoes_f2=6, aprovacoes_f2=4.
- **Etapas testáveis acordadas**: E1 colunas+contagens (APROVADA) → E2 cartão completo (APROVADA) → E3 filtros (APROVADA) → E4 Jornada (APROVADA, "ok bateu") → E5 arrastar com transição (entregue).
- **Verificação de integridade documental** (pedido do owner): troca pontual com conferência de contagens antes/depois (campos, seções `## `, D1–D6, histórico) e publicação por `push_files` quando o arquivo passa de ~16 KB, porque `create_or_update_file` trunca em silêncio nesse tamanho.

## F3-T10 — execução em etapas

**Condição 4 verificada antes de alterar o app — recuperação existe e é verificável:**
- O app é versionado pelo Skip: cada apply gera uma versão (v0.0.146 antes da T10; v0.0.147 E1; v0.0.150 E2; v0.0.152 E3; v0.0.153 E4; v0.0.158 E5). A interface do Skip permite restaurar versão anterior, e a ferramenta de publicação recusa aplicar sobre uma versão restaurada sem `confirmPrune`.
- A mudança é **aditiva**: arquivos novos de serviço e página, 1 import + 1 `<Route path="/funil">` no `App.tsx` e 1 item de menu no `Layout.tsx`. Remover essas linhas desativa a tela e devolve o app ao estado anterior, sem tocar em nenhum dado.
- **Nenhuma migration de estrutura foi criada na T10** — as 0036/0037/0038 eram rotinas de limpeza de resíduo de teste, não alteram schema. O rollback de banco (destrutivo) não é necessário nem será usado.

**E1 — APROVADA (v0.0.147):** rota `/funil` + item "Funil" no menu; 6 colunas de estados ativos + "Encerrados"; contagem por coluna calculada da mesma lista dos cartões; contadores de topo; faixa que confere soma das colunas = total da fonte; selo TESTE; somente leitura. Homologação: "E1 aprovada" (04/11 20:03).

**E2 — APROVADA (v0.0.150):** cartão completo com empresa, tipo de origem, canal, campanha, serviço, área, dono, próxima ação, prazo (vencido em destaque), contagem de propostas/vagas e selo TESTE. Homologação: "tudo confere" (04/11 21:00).
Dois defeitos reais corrigidos: (1) **zero falso sem sessão** — sem sessão o PocketBase devolve lista VAZIA com HTTP 200 (não erro), então o `try/catch` nunca disparava e a tela mostrava "0 propostas · 0 vagas" e "área: não informada" como dado real; corrigido conferindo `pb.authStore.isValid` ANTES de ler os filhos; (2) **`area` não existe em `demandas`** — o campo lá é `segmento`; a área funcional é lida das propostas/vagas por `demanda_id`, nunca inventada.

**E3 — APROVADA (v0.0.152):** filtros de período (por prazo), tipo de origem, canal, campanha, serviço, dono e "incluir dados de teste"; botão "Limpar"; contador "mostrando N de TOTAL". Regra: **o filtro é aplicado ANTES de contar**. Homologação: "ok bateu" (05/11 10:29).
Defeito real corrigido na E3: o contador "Prazo vencido" era o único que lia o dado bruto em vez do filtrado — com o filtro "desconhecido" a tela mostrava **"Demandas 2" e "Prazo vencido 9"** ao mesmo tempo; corrigido. Achado no print do owner.
Decisão declarada: o filtro "incluir dados de teste" começa **marcado** (RN-F3-023 fala de *indicadores oficiais*; o Funil é quadro operacional; 13 de 13 registros são sintéticos).

**E4 — APROVADA (v0.0.153):** clicar no cartão abre o painel **Jornada** com (1) de onde veio; (2) linha do tempo do histórico, em ordem cronológica, com data/hora de Brasília, de→para, autor, entidade e origem; (3) propostas e vagas ligadas. Fontes: rota `/backend/v1/f3-t01/jornada` e coleções `propostas`/`vagas`. Homologação: "ok bateu" (05/11 10:29).
Limite declarado na tela: `campanha` é texto livre, sem vínculo com `campanhas_meta` — a tela escreve isso ao lado do nome.

**E5 — ENTREGUE, aguardando homologação (v0.0.158, ref 3823250):** arrastar o cartão para outra coluna abre o diálogo de mudança de etapa, com **evidência obrigatória** (botão desativado com o campo vazio). A mudança passa pela transição que já existe (RN-F3-021) — o Funil não tem mecanismo próprio. Recusa mostra o motivo exato do servidor e o cartão não se move. Só envia `estado` + `evidencia`; os demais campos permanecem.
**Defeito real corrigido na E5:** a primeira publicação (v0.0.157) deixou a tela **em branco** — `confirmarMover` usava `carregar` na lista de dependências antes de `carregar` ser declarado. Erro de EXECUÇÃO que o build do Skip não pega. Corrigida a ordem dos hooks; 13 cartões arrastáveis conferidos no navegador.
**Teste de recusa, sem gerar evento:** PATCH em `FIX-IN-001` (suspect → prospect) recusado com "Nao e possivel avancar para Prospect: handoff_status=aceito e obrigatorio"; estado permaneceu `suspect`; `pipeline_log` continua em 4 — nenhum evento novo.

## F3-T10 — E5: verificação do resíduo e decisão do owner (05/11/2026)

**Decisão do owner (05/11 11:02):** preservar os dois eventos de teste no histórico. **Não autorizada** exclusão por superusuário nem novas tentativas de limpeza. Registro formal publicado em `06_notas/registro-eventos-teste-e5-2026-11-05.md`.

**As cinco verificações pedidas, todas cumpridas:**

1. **Identificadores confirmados na fonte:** demanda `PIPE-F3-STUCK-001` (id interno `l4ctzbl1qa1l40w`); eventos `qdn60rs36n8i801` (oportunidade→proposta, 13:45:59Z) e `nspvdbuvc3m37oc` (proposta→oportunidade, 13:46:14Z). Ambos `origem_dado=sintetico`.
2. **Efeito das 3 migrations: nenhum no schema.** 0036/0037/0038 rodaram como "applied" e **nenhuma apagou evento**; nenhuma criou ou alterou coleção, campo, índice ou regra — as 13 coleções seguem iguais. A 0037 teve um efeito desejado: limpou a `evidencia` de teste da demanda. Causa raiz das três falhas: `getString('created')` em campo de **data** devolve vazio neste runtime, então a comparação nunca casava e a migration reportava sucesso sem limpar.
3. **Baseline restaurado:** `estado=oportunidade`, `evidencia` vazia, demais campos conferidos um a um e intactos. 13 demandas; estados 9 suspect / 1 prospect / 3 oportunidade — idêntico.
4. **Sem contaminação de indicador oficial:** o painel (Index) lê **só** `demandas` filtrando `origem_dado=producao` e **não lê** `pipeline_log`; há 0 registros de produção; os 2 eventos são sintéticos. O histórico só aparece dentro da Jornada de um cartão, com selo TESTE.
5. **Append-only preservado:** `DELETE` pela API devolve **HTTP 403 "Only superusers can perform this action"**; `deleteRule` e `updateRule` continuam `null`.

**Diferença única do baseline:** `pipeline_log` tem **4** registros em vez de 2 — os dois do teste, preservados por decisão do owner e identificados no registro formal.
