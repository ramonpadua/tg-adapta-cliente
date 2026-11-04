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
- autorizacao_implementacao: CONFIRMADA para a T10 (04/11 17:41) e E1 APROVADA pelo owner em 04/11 20:03 ("E1 aprovada"), o que autoriza a E2. Sequência E1→E5 com parada para homologação após cada etapa; T11 só depois da T10 integral. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12); 2026-10-28 21:31 (agendador); 2026-10-30 (Graph API direta); 2026-11-04 14:30 (testes F3-T13); 2026-11-04 17:35 (retomar T10/T11).
- teste_humano: E1 APROVADA pelo owner em 04/11 20:03 ("E1 aprovada"). E2 ENTREGUE em 04/11 20:20, aguardando homologação. Fases anteriores: F3-T13 homologada 04/11 14:53; F3-T12 em 03/11. T10/T11 pendentes de homologação final: "Não conclua T10/T11 sem os testes previstos e minha homologação".
- verificacao_automatica: E1 PASSOU (v0.0.147) e E2 PASSOU (v0.0.150, ref 98332fb), QA setup/static/build/integrations OK nas três publicações da E2. Contagem conferida contra a fonte: suspect 9, prospect 1, oportunidade 3, demais 0; soma 13 = fonte 13. DOIS DEFEITOS REAIS achados e corrigidos na E2: (1) sem sessão, `propostas`/`vagas` devolvem lista VAZIA com HTTP 200 (não erro) — a tela mostrava "0 propostas · 0 vagas" e "área: não informada" como se fosse dado real, quando PIPE-F3-IN-001 tem 1 proposta e 1 vaga; o try/catch nunca disparava. Corrigido conferindo `pb.authStore.isValid` ANTES de ler os filhos: sem sessão, a tela avisa "Propostas, vagas e área não puderam ser lidas" e cada cartão mostra "não foi possível ler"; (2) `area` não existe em `demandas` (o campo lá é `segmento`, setor da empresa) — a área funcional é lida das propostas/vagas por `demanda_id`, nunca inventada. BASELINE PRESERVADO: demandas 13, propostas 2, vagas 2, pipeline_log 2, campanhas_meta 130, experimentos_f2 3, decisoes_f2 6, aprovacoes_f2 4 — idêntico; conexao_meta 114→115 por execução do cron de campanhas às 22:00 UTC (19:00 BRT), não pela T10. REGRESSÃO OK: /pipeline-t01 com PIPE-F3-IN/OUT/STUCK e botões de retorno; /campanhas com "Conexão com a Meta ativa" e as 10 campanhas.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-04 20:20 — F3-T10 ETAPA E2 implementada e entregue para teste (v0.0.150): cartão completo com empresa, origem, canal, campanha, serviço, área, dono, próxima ação, prazo (vencido em destaque), contagem de propostas/vagas e selo TESTE; mais o aviso honesto quando propostas/vagas não são legíveis sem sessão. E1 aprovada antes.
- proxima_acao: aguardar homologação da E2 pelo owner; não iniciar a E3 (filtros) sem a confirmação dele
- atualizado_em: 2026-11-04T20:20:00-03:00

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

## Análise da F3-T10/T11 (04/11/2026) — entregue, não implementada

- **Autorização do owner (04/11 17:35)**: "Autorizo retomar exclusivamente a F3-T10/T11, referente à tela Funil"; "avançar sem aguardar a matriz B3-RACI-01, desde que ela não seja uma dependência obrigatória da T10/T11"; "Execute em etapas testáveis"; "Não conclua T10/T11 sem os testes previstos e minha homologação".
- **B3-RACI-01 NÃO bloqueia a T10/T11** — conferido na SPEC-F3-005: a matriz é exigida na F3-T02 (prova de equivalência) e na F3-T05/T06 (handoff). A T10/T11 não depende dela.
- **O código do app NÃO está no GitHub** (o repo `ramonpadua/tg-adapta-cliente` guarda só a documentação: 01_projeto…06_notas). O código vive no projeto Skip 51414, versionado por apply (v0.0.146). Preservação se verifica contra o app, não contra o repo.
- **Reaproveitamento total — sem migration**: `demandas` já tem empresa, estado, tipo_origem, canal, campanha, oferta_servico, responsavel, proxima_acao, prazo, origem_dado, evidencia; `propostas` (2) e `vagas` (2) ligam por `demanda_id`; `pipeline_log` (2) tem estado_anterior, estado_novo, evidencia, quando, autor, demanda_id. Falta só a tela.
- **Faixas de estados conferidas**: `suspect`, `prospect`, `lead_qualificado`, `oportunidade`, `proposta`, `vaga_aberta`, terminais `ganho`/`perdido`/`sem_timing`/`desqualificado`.
- **Dado sintético para a prova**: 13 demandas, todas `origem_dado=sintetico`; **2** com `tipo_origem=desconhecido` (FIX-UNK-001 e ATR-F2-UNK-001); outros 6 sem `campanha` e 2 sem `canal`. Cobre o cenário de origem ausente da T11.
- **Achado que exige decisão**: `demandas` não tem campo `tipo` inbound/outbound — usa `tipo_origem`; e `campanha` é texto livre, sem vínculo com `campanhas_meta`. A Jornada mostrará a campanha como texto, não como registro da F3-T12. Registrar como decisão, não inventar ligação.
- **Baseline a preservar (04/11 17:40)**: demandas=13, propostas=2, vagas=2, pipeline_log=2, campanhas_meta=130, conexao_meta=114, experimentos_f2=3, decisoes_f2=6, aprovacoes_f2=4.
- **Etapas testáveis acordadas**: E1 colunas+contagens → E2 cartão completo → E3 filtros → E4 Jornada → E5 arrastar com transição. T11 (as provas) só depois da T10 homologada.
- **Verificação de integridade documental** (pedido do owner): troca pontual com conferência de contagens antes/depois (tasks `- [x]`, seções `## `, linhas de tabela) e publicação por `push_files` quando o arquivo passa de ~16 KB, porque `create_or_update_file` trunca em silêncio nesse tamanho.

## F3-T10 — execução em etapas (autorizada 04/11 17:41)

**Condição 4 verificada antes de alterar o app — recuperação existe e é verificável:**
- O app é versionado pelo Skip: cada apply gera uma versão (antes da E1: v0.0.146 / ref 3e8f321; depois: v0.0.147 / c73a040; E2: v0.0.150 / 98332fb). A interface do Skip permite restaurar versão anterior, e a própria ferramenta refusa aplicar sobre uma versão restaurada sem `confirmPrune` — o que confirma que a restauração existe no fluxo.
- A mudança é **aditiva**: arquivos novos de serviço e página, 1 import + 1 `<Route path="/funil">` no `App.tsx` e 1 item de menu no `Layout.tsx`. Remover essas linhas desativa a tela e devolve o app ao estado anterior, sem tocar em nenhum dado.
- **Nenhuma migration foi criada nesta task** — logo o rollback de banco (destrutivo) não é necessário nem será usado.

**E1 — entregue e APROVADA (v0.0.147, ref c73a040):** rota `/funil` + item "Funil" no menu; 6 colunas de estados ativos + "Encerrados"; contagem por coluna calculada da mesma lista dos cartões; contadores de topo (Demandas, Sem origem, Sem canal, Prazo vencido, De teste); faixa que confere soma das colunas = total da fonte; selo TESTE; somente leitura. **Homologação do owner: "E1 aprovada" (04/11 20:03).**

**E2 — entregue, aguardando homologação (v0.0.150, ref 98332fb):** cartão completo com empresa, tipo de origem, canal, campanha, serviço, área, dono, próxima ação, prazo (vencido em destaque), contagem de propostas/vagas e selo TESTE. Origem/canal/campanha ausentes aparecem como `desconhecido` em âmbar (RN-F3-022), nunca em branco.

**Dois defeitos reais encontrados na E2 (ambos corrigidos):**
1. **Zero falso sem sessão.** Eu supus que, sem sessão, `propostas`/`vagas` dariam erro e cairia no `catch`. Não dão: o PocketBase filtra e devolve HTTP 200 com lista **vazia** (comprovado: sem token, `items=0 total=0`, sem mensagem). O `catch` nunca disparava e a tela mostrava "0 propostas · 0 vagas" e "área: não informada" como dado real — quando `PIPE-F3-IN-001` tem 1 proposta e 1 vaga. Corrigido conferindo `pb.authStore.isValid` **antes** de ler os filhos: sem sessão a tela avisa e o cartão diz "não foi possível ler", em vez de inventar zero.
2. **`area` não existe em `demandas`.** O campo lá é `segmento` (setor da empresa). A área funcional do cargo vive em `propostas`/`vagas` (E-F3-001-01, D2). A leitura passou a vir dos filhos por `demanda_id`; nenhum campo novo foi criado.

**Provas da E2 (04/11 20:20):** logado — 2 cartões com "1 proposta · 1 vaga" (exatamente os dois PIPE-F3 que têm filhas), "área: TI" e "área: Logística/Supply Chain" visíveis, faixa "Contagem confere com a fonte". Sem sessão — aviso no topo e cartões com "não foi possível ler", zero cartão mostrando zero falso. Baseline idêntico; `conexao_meta` subiu 1 linha pelo cron das campanhas às 22:00 UTC, não pela T10. Regressão OK em `/pipeline-t01` e `/campanhas`.
