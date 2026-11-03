# Estado atual — Adapta Cliente

- task_id: nenhuma
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-006-campanhas-meta-maestro.md (concluída)
- fase: 3
- task_elegivel: F3-T01 (próxima; exige autorização antes de implementar)
- tasks_fase_3: 2/13 concluídas (F3-T09, F3-T12); F3-T01 destravada pela emenda E-F3-001-04, próxima a executar
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: sem_task
- autorizacao_implementacao: confirmada — 2026-10-28 10:58, Champion/owner respondeu "SIm" à pergunta "Posso implementar este plano na F3-T12?" (relatório de análise entregue antes, em 28/10 10:05); 2026-10-28 21:31, "vou com sua recomendação" autorizou a conta de serviço dedicada do agendador (caminho A); 2026-10-30, owner escolheu "Caminho C" (comparação primeiro) e depois autorizou a troca de fonte para Graph API direta + reafirmou o aceite ("Sim — mesmo principal, mesmo somente-leitura")
- teste_humano: APROVADO — 2026-10-28, o Champion executou a prova no navegador e enviou o PDF com 26/26 checagens (11 RED + 10 GREEN + 5 regressão); 2026-10-30, o OWNER atestou o CA-3-16 conferindo as 10 campanhas da janela 22–28/10 no Gerenciador de Anúncios ("bateu"); 2026-11-03 17:19, o OWNER reenviou a tela `/campanhas` recarregada e os sete números do último retrato (10 campanhas, 1 ativa, gasto R$ 17,56, 4.663 impressões, 184 cliques, 113 resultados) foram conferidos campo a campo contra a Graph API e batem. Fechamento autorizado pelo decisor
- verificacao_automatica: passou — Skip v0.0.120 (ref 5d5c261) QA setup/static/build/integrations OK. LEITURA REAL EXECUTADA em 30/10 02:47 UTC: campanhas_lidas=10, linhas_gravadas=70, erros=[], somente_leitura_meta=true. TRÊS DEFEITOS REAIS encontrados pela conferência visual do owner e corrigidos: (1) v0.0.118 — `normalizarStatus` convertia o booleano da comparação (String(false)='false') e gravava 'desconhecido' em 80/80 linhas; (2) v0.0.119 — a tela não tinha porta de entrada própria e, sem sessão, mostrava zeros e "Sem informação da conexão" (as coleções exigem `listRule: @request.auth.id != ''`); (3) v0.0.120 — `resultados` somava os 13 itens de `actions` da Meta (engajamento + conversão + duplicata omni_*), mostrando 654 onde o real era 113; agora `escolherResultado()` pega 1 tipo por objetivo. CONFERÊNCIA FINAL (03/11 17:19) contra a Graph API, campo a campo: 10 campanhas, 1 ativa, gasto R$ 17,56, 4.663 impressões, 184 cliques, 113 resultados — todos batem; 9 arquivadas com zero em tudo e orçamentos/dia conferem. Rotina diária estável: 31/10 a 03/11, 70 linhas por execução, zero erro. Fonte: Graph API v21.0 direta, token no segredo META_ACCESS_TOKEN (principal João Paulo Oliveira, ads_read, sem expiração), somente GET.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-03 17:19 — F3-T12 CONCLUÍDA. Owner conferiu a tela `/campanhas` recarregada e os sete números batem com a Graph API; terceiro defeito (campo `resultados` somando 13 tipos de `action`) corrigido em v0.0.120. Antes: 2026-10-30 03:18 — defeito real corrigido (v0.0.118, ref edcd38e) após conferência visual do owner; `normalizarStatus` convertia o booleano da comparação em vez do valor e gravava 'desconhecido' em 80/80 linhas. Antes disso: fonte trocada para Graph API direta (v0.0.117), leitura real executada, CA-3-16 atestado pelo owner
- proxima_acao: apresentar o plano da F3-T01 e pedir autorização antes de implementar (migrations 0026/0027: `propostas`, `vagas` e log append-only; materializar os 3 fixtures; regras de retorno e escalonamento). F3-T13 destrava em paralelo quando o Champion quiser
- atualizado_em: 2026-11-03T17:19:00-03:00

## Defaults conservadores da F3-T01 (protocolo anti-trava — Constituição §Dúvidas e insumos, 07/10/2026)

Adotados pelo Champion em 09/10/2026 enquanto as emendas não chegam; nenhuma DÚVIDA é considerada resolvida:

- **D1 (vocabulário R&S/Alocação/SOS):** fixtures usam exclusivamente R&S / TMO / R&S + TMO como valores de `oferta_servico` (decisão Champion 07/10, changelog); "R&S/Alocação/SOS" é lido como descrição da operação que devolve o retorno, não valor de campo. SOS = Serviço Orientado a Substituições, dentro do TMO.
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
