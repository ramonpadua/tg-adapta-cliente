# Estado atual — Adapta Cliente

- task_id: F3-T12
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-006-campanhas-meta-maestro.md
- fase: 3
- task_elegivel: F3-T12 (selecionada)
- tasks_fase_3: 1/13 concluídas (F3-T09); F3-T12 com leitura real executada e CA-3-16 ATESTADO pelo owner em 30/10 — aguardando confirmação do teste humano para concluir; F3-T01 destravada pela emenda E-F3-001-04 (não é a task ativa)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada — 2026-10-28 10:58, Champion/owner respondeu "SIm" à pergunta "Posso implementar este plano na F3-T12?" (relatório de análise entregue antes, em 28/10 10:05); 2026-10-28 21:31, "vou com sua recomendação" autorizou a conta de serviço dedicada do agendador (caminho A); 2026-10-30, owner escolheu "Caminho C" (comparação primeiro) e depois autorizou a troca de fonte para Graph API direta + reafirmou o aceite ("Sim — mesmo principal, mesmo somente-leitura")
- teste_humano: parcialmente aprovado — 2026-10-28, o Champion executou a prova no navegador e enviou o PDF com 26/26 checagens (11 RED + 10 GREEN + 5 regressão); 2026-10-30, o OWNER atestou o CA-3-16 ("bateu") conferindo as 10 campanhas da janela 22–28/10 no Gerenciador de Anúncios. Falta a confirmação explícita do Champion (ou do owner como decisor) para o fechamento formal da task
- verificacao_automatica: passou — Skip v0.0.118 (ref edcd38e) QA setup/static/build/integrations OK; corrigido DEFEITO REAL encontrado na conferência visual do owner: `normalizarStatus` no hook fazia String(v===undefined||v===null||v==='') — convertia o resultado booleano da comparação, String(false)='false', e as 80 linhas foram gravadas com status_efetivo='desconhecido'. Depois do fix, nova execução real: 10 campanhas, 70 linhas, 1 active + 9 archived corretos; prova RED/GREEN/REGRESSÃO no navegador 26/26 (28/10); LEITURA REAL EXECUTADA em 30/10 02:47 UTC pela rota interna: campanhas_lidas=10, linhas_gravadas=70 (10 campanhas × 7 dias), criadas=0, atualizadas=70, erros=[], somente_leitura_meta=true, janela 22–28/10. Fonte: Graph API v21.0 direta, token no segredo META_ACCESS_TOKEN (principal João Paulo Oliveira, ads_read, sem expiração). Aceite da nova fonte reafirmado pelo owner em 30/10 e publicado em 06_notas/aceite-graph-api-f3-t12-2026-10-30.md (commit 3435693)
- aprendizado: pendente
- ultima_acao: 2026-10-30 03:18 — F3-T12: defeito real corrigido (v0.0.118, ref edcd38e) após conferência visual do owner. `normalizarStatus` convertia o booleano da comparação em vez do valor e gravava 'desconhecido' em 80/80 linhas; corrigido e reexecutado (1 active + 9 archived). Evidência atualizada em 05_entregas/campanhas-f3.md (commit be3b61d). Antes: fonte trocada para Graph API direta (v0.0.117), leitura real executada, CA-3-16 atestado pelo owner
- proxima_acao: confirmar com o Champion/owner o fechamento formal da F3-T12 (teste humano completo, agora com a tela exibindo a situação correta); depois, F3-T13 (provas de falha, idempotência e destino único) destrava
- atualizado_em: 2026-10-30T00:20:00-03:00

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

Esta atualização não muda a task ativa F3-T12, não conclui F3-T01 nem comprova E2-coleções/E5/E6. Histórico técnico de 28/10 e aceites anteriores preservados integralmente. O Champion encerra T12 ou registra pausa segura antes de retomar T01; uma task por vez. E4 continua plano sem emenda/task liberada; dados pessoais reais continuam condicionados à política LGPD aprovada.
