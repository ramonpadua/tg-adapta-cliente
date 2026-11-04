# Estado atual — Adapta Cliente

- task_id: F3-T13
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-006-campanhas-meta-maestro.md (concluída)
- fase: 3
- task_elegivel: F3-T13 (selecionada; já implementada — em teste humano)
- tasks_fase_3: 3/13 concluídas (F3-T09, F3-T12, F3-T01); F3-T13 implementada e aguardando homologação do teste revisado
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada — 2026-10-28 10:58, Champion/owner respondeu "SIm" à pergunta "Posso implementar este plano na F3-T12?" (relatório de análise entregue antes, em 28/10 10:05); 2026-10-28 21:31, "vou com sua recomendação" autorizou a conta de serviço dedicada do agendador (caminho A); 2026-10-30, owner escolheu "Caminho C" (comparação primeiro) e depois autorizou a troca de fonte para Graph API direta + reafirmou o aceite ("Sim — mesmo principal, mesmo somente-leitura"); 2026-11-04 14:30, owner autorizou refazer os testes da F3-T13 com o token oficial ("agora refaça os testes da tarefa usando esse token oficial")
- teste_humano: APROVADO (F3-T12) — 2026-10-28, o Champion executou a prova no navegador e enviou o PDF com 26/26 checagens (11 RED + 10 GREEN + 5 regressão); 2026-10-30, o OWNER atestou o CA-3-16 conferindo as 10 campanhas da janela 22–28/10 no Gerenciador de Anúncios ("bateu"); 2026-11-03 17:19, o OWNER reenviou a tela `/campanhas` recarregada e os sete números do último retrato foram conferidos campo a campo contra a Graph API e batem. PENDENTE (F3-T13) — testes refeitos pelo Maestro em 04/11 com o token oficial; aguardando o owner repetir na tela `/campanhas/prova-conexao` e confirmar
- verificacao_automatica: passou — Skip v0.0.142 (F3-T13). Bateria de 04/11 com o token oficial: leitura real 10 campanhas / 70 linhas / 0 erros / 0 criadas; os 5 cenários de ensaio gravaram 0 linhas e NÃO alteraram a saúde (107→107); reexecução 0 criadas e 70 atualizadas; 130 linhas e 0 pares duplicados; auditoria `escrita_na_meta=0` e destino único OK. Conferência contra a fonte em 03/11: gravado R$ 15,68 / 3.068 impressões / 162 cliques / 105 resultados = Graph API idêntica. Regressão: F3-T01 intacta (propostas 2, vagas 2, pipeline_log 2), demandas 13 todas `origem_dado=sintetico`. Histórico anterior (F3-T12): v0.0.120, ref 5d5c261, três defeitos reais corrigidos (normalizarStatus, porta de entrada, campo `resultados`).
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-04 14:45 — F3-T13 com testes refeitos com o token oficial (v0.0.142). Achado um defeito real durante os testes: o modo ensaio gravava saúde e disparava e-mail. Antes: 2026-11-03 17:19 — F3-T12 CONCLUÍDA com conferência humana da tela contra a Graph API
- proxima_acao: owner repetir os testes da F3-T13 na tela `/campanhas/prova-conexao` com o token oficial (v0.0.142) e confirmar para concluir
- atualizado_em: 2026-11-04T14:45:00-03:00

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

Esta atualização não muda o resultado acima (F3-T12 concluída em 03/11), não conclui F3-T01 nem comprova E2-coleções/E5/E6. Histórico técnico de 28/10 e aceites anteriores preservados integralmente. Com a T12 concluída, a próxima é a F3-T01, que exige autorização do Champion antes de implementar; uma task por vez. E4 continua plano sem emenda/task liberada; dados pessoais reais continuam condicionados à política LGPD aprovada.

## Ciclo da F3-T13 — testes refeitos com o token oficial (04/11/2026)

- **Pedido do owner**: "esse token até já criou campanhas, excluiu campanhas inativas. é só ele que deve ficar aqui ok? agora refaça os testes da tarefa usando esse token oficial".
- **Credencial conferida**: o token do segredo `META_ACCESS_TOKEN` e o arquivo `credentials/meta_token.json` são o MESMO (impressão digital sha256 `7080ebf8c792` nos dois), principal João Paulo Oliveira, sem expiração. Verificação por impressão digital, sem expor o valor.
- **Correção de afirmação errada**: a rota `f3-t13/token-info` dizia `escrita: false` ao lado do token, dando a entender que o token não escreve. O owner esclareceu que esse token já criou e excluiu campanhas. Agora a rota separa `token_pode_escrever_na_meta: true` (poder da credencial) de `automacao_escreve_na_meta: false` (comportamento do código).
- **DEFEITO REAL encontrado ao rodar os testes (v0.0.140)**: o modo ensaio chamava `registrarSaude()` e `avisarChampion()`. Rodar "Rodar os cinco" gravou 6 linhas de 'erro' em `conexao_meta` e disparou e-mail de "Conexão Meta expirada" com a conexão saudável; a tela passou a mostrar "Saúde da conexão: erro". Corrigido: ensaio não grava saúde nem envia e-mail. Resíduo removido pela migration 0035 (apaga só linhas cuja mensagem começa com "Ensaio").
- **Bateria final (v0.0.142), toda aprovada**: leitura real 10 campanhas/70 linhas/0 erros; 5 cenários de ensaio com 0 linhas gravadas e sem alterar a saúde (107→107); reexecução com 0 criadas e 70 atualizadas; 130 linhas e 0 pares duplicados; auditoria com escrita_na_meta=0 e destino único OK.
- **Conferência contra a fonte (03/11)**: gravado R$ 15,68 / 3.068 impressões / 162 cliques / 105 resultados = Graph API idêntica.
- **Regressão**: F3-T01 intacta (propostas 2, vagas 2, pipeline_log 2), demandas 13 todas `origem_dado=sintetico`.
- **Pendência**: homologação do owner no teste revisado. Um e-mail falso de "Conexão Meta expirada" chegou à caixa do Champion por causa do defeito — informado ao owner.
