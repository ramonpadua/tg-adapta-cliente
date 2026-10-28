# Estado atual — Adapta Cliente

- task_id: F3-T12
- champion: João Paulo
- spec: 04_fase-atual/specs/spec-f3-006-campanhas-meta-maestro.md
- fase: 3
- task_elegivel: F3-T12 (selecionada)
- tasks_fase_3: 1/13 concluídas (F3-T09); F3-T12 IMPLEMENTADA — aguardando teste humano; F3-T01 destravada pela emenda E-F3-001-04 (não é a task ativa)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada — 2026-10-28 10:58, Champion/owner respondeu "SIm" à pergunta "Posso implementar este plano na F3-T12?" (relatório de análise entregue antes, em 28/10 10:05)
- teste_humano: pendente (roteiro entregue 28/10 11:05; nada concluído antes da confirmação)
- verificacao_automatica: passou — Skip v0.0.101 (ref 06fb4ca) QA setup/static/build/integrations OK; prova RED/GREEN/REGRESSÃO executada no navegador 26/26 (1a rodada 25/26 com falha real corrigida: núcleo não lia `actions` e contava aliases como descartados); coleções campanhas_meta (índice único campaign_id+data_referencia, escrita bloqueada na API) e conexao_meta criadas; rotas /campanhas e /campanhas/prova publicadas no preview. NÃO executado: leitura real contra a Meta (token do conector vencido desde 19/10 e conta de serviço do agendador não configurada)
- aprendizado: pendente
- ultima_acao: 2026-10-28 11:05 — F3-T12 IMPLEMENTADA (Skip v0.0.101, ref 06fb4ca): migrations 0022 (campanhas_meta + conexao_meta) e 0023 (cron pausado); núcleo puro + prova; telas /campanhas e /campanhas/prova; automação campanhas_meta_sync + _run + _cron
- proxima_acao: teste humano do Champion no preview (/campanhas e /campanhas/prova) e confirmação para concluir
- atualizado_em: 2026-10-28T11:05:00-03:00

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