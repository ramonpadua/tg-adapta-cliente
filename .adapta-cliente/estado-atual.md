# Estado atual — Adapta Cliente

- task_id: F3-T10
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-005-funil-kanban.md
- fase: 3
- task_elegivel: F3-T10 (selecionada; F3-T11 vem depois dela, bloqueada até a T10 ser aceita)
- tasks_fase_3: 4/13 concluídas (F3-T09, F3-T12, F3-T01, F3-T13)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_autorizacao
- autorizacao_implementacao: AUSENTE para a T10. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12, "SIm"); 2026-10-28 21:31 (conta de serviço do agendador); 2026-10-30 (fonte Graph API direta); 2026-11-04 14:30 (refazer testes da F3-T13 com o token oficial); 2026-11-04 17:35 ("Autorizo retomar exclusivamente a F3-T10/T11, referente à tela Funil" — pede análise antes de implementar e etapas testáveis)
- teste_humano: APROVADO até a F3-T13 (F3-T12: PDF 26/26 em 28/10, CA-3-16 atestado em 30/10, tela conferida em 03/11; F3-T13: homologada em 04/11 14:53 — "Aprovado — pode concluir a T13"). PENDENTE para T10/T11: o owner definiu "Não conclua T10/T11 sem os testes previstos e minha homologação".
- verificacao_automatica: pendente para a T10 (nada implementado). Passou na F3-T13: v0.0.146 — prova de falha real (e-mail confirmado no log, faixa na tela, 130 linhas intactas, zero escrita na Meta), 0 criadas/70 atualizadas, 0 duplicados. Baseline a revalidar a cada etapa da T10: demandas=13, propostas=2, vagas=2, pipeline_log=2, campanhas_meta=130, conexao_meta=114, experimentos_f2=3, decisoes_f2=6.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-04 17:40 — análise da F3-T10/T11 ENTREGUE, nada implementado. Antes: 15:02 — F3-T13 CONCLUÍDA (v0.0.146), verificação refeita do zero, quatro defeitos reais corrigidos.
- proxima_acao: aguardar autorização para implementar a T10 — análise entregue em 04/11 17:40; T11 só inicia após T10 aceita por teste humano
- atualizado_em: 2026-11-04T17:40:00-03:00

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
- **Dado sintético para a prova**: 13 demandas, todas `origem_dado=sintetico`; 3 com `tipo_origem=desconhecido` (FIX-UNK-001, ATR-F2-UNK-001 e sem canal/campanha) — cobre o cenário de origem ausente da T11.
- **Achado que exige decisão**: `demandas` não tem campo `tipo` inbound/outbound — usa `tipo_origem`; e `campanha` é texto livre, sem vínculo com `campanhas_meta`. A Jornada mostrará a campanha como texto, não como registro da F3-T12. Registrar como decisão, não inventar ligação.
- **Baseline a preservar (04/11 17:40)**: demandas=13, propostas=2, vagas=2, pipeline_log=2, campanhas_meta=130, conexao_meta=114 (após limpeza do ensaio), experimentos_f2=3, decisoes_f2=6.
- **Etapas testáveis acordadas**: E1 colunas+contagens → E2 cartão completo → E3 filtros → E4 Jornada → E5 arrastar com transição. T11 (as provas) só depois da T10 homologada.
- **Verificação de integridade documental** (pedido do owner): adotado procedimento de troca pontual com conferência de contagens antes/depois (tasks `- [x]`, seções `## `, linhas de tabela) e publicação por `push_files` quando o arquivo passa de ~16 KB, porque `create_or_update_file` trunca em silêncio nesse tamanho.
