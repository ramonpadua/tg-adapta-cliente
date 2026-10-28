# Estado atual — Adapta Cliente

- task_id: F3-T12
- champion: João Paulo
- spec: 04_fase-atual/specs/spec-f3-006-campanhas-meta-maestro.md
- fase: 3
- task_elegivel: F3-T12 (selecionada)
- tasks_fase_3: 1/13 concluídas (F3-T09); F3-T12 analisada — aguardando autorização; F3-T01 destravada pela emenda E-F3-001-04 (não é a task ativa)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: aguardando_autorizacao
- autorizacao_implementacao: ausente (o Champion escolheu a frente "tela de campanhas" em 28/10 09:58; a SKILL exige autorização explícita do plano em mensagem separada)
- teste_humano: pendente
- verificacao_automatica: baseline inspecionado em 28/10 — projeto 51414 tem 8 coleções (campanhas_meta/conexao_meta NÃO existem), rotas sem /campanhas, migration mais recente 0021 (próxima = 0022), SMTP no relay da plataforma (noreply@mail.goskip.dev), segredo COMPOSIO_API_KEY presente, conector OAuth do Skip disponível no app (src/lib/skipConnectors.ts); MCP metaads com token EXPIRADO desde 19/10 (401 confirmado hoje — leitura de campanhas não executável por essa via)
- aprendizado: pendente
- ultima_acao: 2026-10-28 10:05 — análise profunda da F3-T12 concluída (SPEC F3-006 inteira, plano v2 §1–14, código do 51414, coleções ao vivo, migrations, SMTP, segredos); nada implementado
- proxima_acao: aguardar autorização do Champion para implementar a F3-T12
- atualizado_em: 2026-10-28T10:05:00-03:00

## Defaults conservadores da F3-T01 (protocolo anti-trava — Constituição §Dúvidas e insumos, 07/10/2026)

Adotados pelo Champion em 09/10/2026 enquanto as emendas não chegam; nenhuma DÚVIDA é considerada resolvida:

- **D1 (vocabulário R&S/Alocação/SOS):** fixtures usam exclusivamente R&S / TMO / R&S + TMO como valores de `oferta_servico` (decisão Champion 07/10, changelog); "R&S/Alocação/SOS" na SPEC é lido como descrição da operação que devolve o retorno, não valor de campo. SOS = Serviço Orientado a Substituições, dentro do TMO.
- **D2 (área + lista v1):** área = departamento funcional do cargo; lista fechada v1 com 13 áreas (TI, Comercial/Vendas, Logística/Supply Chain, Projetos/PMO, Engenharia/Manutenção, Financeiro/Contábil, Administrativo, Marketing, Recursos Humanos, Operações/Produção, Educacional/Acadêmico, Auditoria/Riscos, Laboratorial/Saúde); "Não informado" = condição provisória, não área definitiva (DECISÃO 08/10).
- **D3 (RN-F3-002 × Não informado):** área "Não informado" não bloqueia avanço e não gera pendência automática nova; confirmação humana responsável (DECISÃO 08/10).
- **D4 (persistência do estado do retorno):** EMENDADO (E-F3-001-04, 26/10) — estado corrente na própria vaga + cada alteração também no log append-only; default conservador não é mais necessário; E2 liberada.
- **D5 (encerramento da substituição):** EMENDADO (E-F3-001-04, 26/10) — "Substituição encerrada" aprovado com motivo obrigatório ("reposto" ou "encerrada sem reposição"); não encerra a vaga automaticamente; E5 liberada.
- **D6 (regras de recuperação/RN-F3-004):** EMENDADO (E-F3-001-04, 26/10) — regras de 24/09 incorporadas ao corpo da SPEC (3 tentativas, 90 dias corridos, sugestão de "sem timing" com confirmação expressa do Comercial, nada automático); E6 liberada.

## DÚVIDAs documentais pendentes (lote único ao consultor)

NENHUMA — D1–D6 resolvidas pela emenda E-F3-001-04 (26/10/2026), conferida 15/15 em 28/10/2026. Registros históricos das dúvidas permanecem no changelog (07–08/10 e 24–26/10) como registro do processo.