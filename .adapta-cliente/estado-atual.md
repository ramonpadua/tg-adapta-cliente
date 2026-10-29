# Estado atual — Adapta Cliente

- task_id: F3-T12
- champion: João Paulo
- spec: 04_fase-atual/specs/spec-f3-006-campanhas-meta-maestro.md
- fase: 3
- task_elegivel: F3-T12 (selecionada)
- tasks_fase_3: 1/13 concluídas (F3-T09); F3-T12 IMPLEMENTADA, prova humana 26/26 — CA-3-16 aberto (falta leitura real); F3-T01 destravada pela emenda E-F3-001-04 (não é a task ativa)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada — 2026-10-28 10:58, Champion/owner respondeu "SIm" à pergunta "Posso implementar este plano na F3-T12?" (relatório de análise entregue antes, em 28/10 10:05); 2026-10-28 21:31, "vou com sua recomendação" autorizou a conta de serviço dedicada do agendador (caminho A)
- teste_humano: parcialmente aprovado — 2026-10-28, o Champion executou a prova no navegador e enviou o PDF com 26/26 checagens (11 RED + 10 GREEN + 5 regressão). CA-3-16 CONTINUA ABERTO: comparar com o Gerenciador de Anúncios exige a leitura real, que depende da reconexão Meta
- verificacao_automatica: passou — Skip v0.0.101 (ref 06fb4ca) QA setup/static/build/integrations OK; prova RED/GREEN/REGRESSÃO no navegador 26/26 (1a rodada 25/26 com falha real corrigida: núcleo não lia `actions`); Skip v0.0.102 (migration 0024, conta de serviço) e v0.0.103 (fix do header da rota interna) com QA OK; autenticação da conta de serviço comprovada por HTTP 200 (verified true, role vazio); rota interna exercitada ponta a ponta por curl (antes 401, depois 200); e-mail de falha comprovado no log de runtime (`status: sent`, assunto "Campanhas Meta: Conexao Meta expirada"); `conexao_meta` com 1 registro status=expirada e `campanhas_meta` com 0 — nada inventado na falha. NÃO executado: leitura real contra a Meta (token vencido desde 19/10/2026 10:00 PDT, erro 190/463)
- aprendizado: pendente
- ultima_acao: 2026-10-28 21:36 — F3-T12: infraestrutura de aviso e agendador consertada e comprovada (Skip v0.0.103). Segredo CHAMPION_EMAIL criado (jpaulo@talentgroup.com.br); remetente ajustado para "Talent Group · Adapta"; MAESTRO_INTERNAL_KEY criado (não existia); migration 0024 criou a conta de serviço do agendador com e-mail/senha via $secrets e role vazio (escopo mínimo); corrigido bug real da rota interna (cabeçalho chega como x_internal_key, não X-Internal-Key)
- proxima_acao: reconectar a conta Meta pelo consentimento OAuth do conector; depois executar a leitura real e comparar com o Gerenciador de Anúncios na mesma data para fechar o CA-3-16
- atualizado_em: 2026-10-28T21:36:00-03:00

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
