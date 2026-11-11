# Estado atual — Adapta Cliente

- task_id: nenhuma
- champion: João Paulo
- spec: 04-fase-atual/specs/spec-f3-005-funil-kanban.md
- fase: 3
- task_elegivel: F3-T11 (SUSPENSA até concluir a correção de segurança; exige autorização do Champion)
- tasks_fase_3: 5/13 concluídas (F3-T09, F3-T12, F3-T01, F3-T13, F3-T10)
- prazo_referencia: ajustável pelo Champion (referência inicial 30/09/2026 vencida; sem trava de prazo)
- ambiente: Skip (GoSkip + SkipCloud) — projeto 51414 "Repositório Adapta"
- etapa: bloqueada
- autorizacao_implementacao: SEGURANÇA autorizada pelo owner em 11/11 12:51 (caminho B), 6 etapas, uma por vez com homologação. ETAPA 1 HOMOLOGADA pelo owner em 11/11 16:21. ETAPA 2 aplicada e entregue, **NÃO homologada**: owner relatou páginas em branco às 17:16, diagnóstico entregue. F3-T11 suspensa. Autorizações anteriores preservadas: 2026-10-28 10:58 (F3-T12); 2026-10-28 21:31 (agendador); 2026-10-30 (Graph API direta); 2026-11-04 14:30 (testes F3-T13); 2026-11-04 17:35 (retomar T10/T11).
- teste_humano: F3-T10 APROVADA (5 etapas). SEGURANÇA ETAPA 1 HOMOLOGADA em 11/11 16:21. SEGURANÇA ETAPA 2 NÃO homologada: owner relatou páginas em branco às 17:16. Diagnóstico em `06_notas/diagnostico-paginas-em-branco-2026-11-11.md`: causa comprovada = endereço de PRODUÇÃO acessado, e o app nunca foi publicado (`isPublished: false`). No preview as 3 rotas carregam. Aguardando confirmação do owner sobre o endereço. F3-T13 homologada 04/11 14:53; F3-T12 em 03/11.
- verificacao_automatica: SEGURANÇA etapa 1 HOMOLOGADA (v0.0.159). SEGURANÇA etapa 2 APLICADA em v0.0.160 (ref af1622a). Diagnóstico das páginas em branco (17:20): PREVIEW funciona — `/funil` root 101.964 B com 13 cartões, `/` root 61.478 B, `/pipeline-t01` root 20.435 B, todos em sessão limpa. PRODUÇÃO devolve "Produção não publicada" com root de 0 byte. Arquivos: 4 rotas com HTML 1.268 B apontando `assets/index-DQVPeI_P.js`, que responde 200 com 2.028.699 bytes. Dados intactos: demandas 13, propostas 2, vagas 2, pipeline_log 9, campanhas_meta 200, experimentos_f2 3, decisoes_f2 6; 1 conta champion. Proteções ativas: escrita anônima em demandas 404, criação anônima de conta 403, `role` no pedido 404, leitura anônima de demandas 13 registros (aberta até a etapa 5).
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-03-1719-conferencia-visual-campo-calculado.md
- ultima_acao: 2026-11-11 17:20 — diagnóstico das páginas em branco entregue. Causa comprovada: o endereço de produção foi acessado e o app nunca foi publicado; o preview carrega as três rotas. Nada alterado; proteções e dados intactos.
- proxima_acao: aguardar do owner a confirmação do endereço usado. Se foi produção, fechar o diagnóstico e homologar a etapa 2; se foi preview, o problema é outro e é necessário print com a barra de endereço.
- atualizado_em: 2026-11-11T17:20:00-03:00

## Defaults conservadores da F3-T01 (protocolo anti-trava — Constituição §Dúvidas e insumos, 07/10/2026)

Adotados pelo Champion em 09/10/2026 enquanto as emendas não chegam; nenhuma DÚVIDA é considerada resolvida:

- **D1 (vocabulário R&S/Alocação/SOS):** fixtures usam exclusivamente R&S / TMO / R&S + TMO como valores de `oferta_servico` (decisão Champion 07/10, changelog); "R&S/Alocação/SOS" na SPEC é lido como descrição da operação que retorna o retorno, não valor de campo. SOS = Serviço Orientado a Substituições, dentro do TMO.
- **D2 (área + lista v1):** área = departamento funcional do cargo; lista fechada v1 com 11 áreas (TI, Comercial/Vendas, Logística/Supply Chain, Projetos/PMO, Engenharia/Manutenção, Financeiro/Contábil, Administrativo, Marketing, Recursos Humanos, Operações/Produção, Educacional/Acadêmico, Auditoria/Riscos, Laboratorial/Saúde); "Não informado" = condição provisória, não área definitiva (DECISÃO 08/10).
- **D3 (RN-F3-002 × Não informado):** área "Não informado" não bloqueia avanço e não gera pendência automática nova; confirmação humana responsável (DECISÃO 08/10).
- **D4 (persistência do estado do retorno):** EMENDADO (E-F3-001-04, 26/10) — estado corrente na própria vaga + cada alteração também no log append-only; E2 liberada.
- **D5 (encerramento da substituição):** EMENDADO (E-F3-001-04, 26/10) — "Substituição encerrada" aprovado com motivo obrigatório ("reposto" ou "encerrada sem reposição"); não encerra a vaga automaticamente; E5 liberada.
- **D6 (regras de recuperação/RN-F3-004):** EMENDADO (E-F3-001-04, 26/10) — regras de 24/09 incorporadas ao corpo da SPEC (3 tentativas, 90 dias corridos, sugestão de "sem timing" com confirmação expressa do Comercial, nada automático); E6 liberada.

## DÚVIDAs documentais pendentes (lote único ao consultor)

NENHUMA — D1–D6 resolvidas pela emenda E-F3-001-04 (26/10/2026), conferida 15/15 em 28/10/2026.


## Consolidação documental publicada em 29/10/2026

SPEC F3-001, fase.md, Tasks Gerais, matriz e fixtures foram alinhados com E-F3-001-04. D4/D5/D6 não são bloqueios documentais vigentes.

Histórico técnico de 28/10 e aceites anteriores preservados integralmente. Uma task por vez; cada uma exige autorização do Champion antes de implementar. E4 continua plano sem emenda/task liberada; dados pessoais reais continuam condicionados à política LGPD aprovada.

## Ciclo da F3-T13 — concluída em 04/11/2026 (v0.0.146)

- **Pedido do owner**: "esse token até já criou campanhas, excluiu campanhas inativas. é só ele que deve ficar aqui ok? agora refaça os testes da tarefa usando esse token oficial".
- **Credencial conferida**: o token do segredo `META_ACCESS_TOKEN` e o arquivo `credentials/meta_token.json` são o MESMO (impressão digital sha256 `7080ebf8c792` nos dois), principal João Paulo Oliveira, sem expiração.
- **Correção de afirmação errada**: a rota `f3-t13/token-info` dizia `escrita: false` ao lado do token. Agora separa `token_pode_escrever_na_meta: true` de `automacao_escreve_na_meta: false`.
- **DEFEITO REAL 1 — o ensaio sujava o estado (v0.0.140)**: ensaio chamava `registrarSaude()` e `avisarChampion()`. Corrigido: ensaio não grava saúde nem envia e-mail. Resíduo removido pela migration 0035.
- **DEFEITO REAL 2 — hora crua em UTC na faixa (v0.0.146)**: criada `formatarInstanteBR()` com 4 casos testados.
- **DEFEITO REAL 3 — token expirado volta HTTP 400/190, não 401 (v0.0.145)**: comprovado contra a Graph API com três tokens inválidos.
- **DEFEITO REAL 4 — textos prometiam o que não acontecia (v0.0.143)**: alinhados ao comportamento real.
- **Verificação independente**: prova de falha real com credencial inválida → e-mail no log, faixa na tela, 130 linhas preservadas; reexecução 0 criadas / 70 atualizadas.
- **Homologação**: "Aprovado — pode concluir a T13" (04/11/2026 14:53).

## Análise da F3-T10/T11 (04/11/2026)

- **Autorização do owner (04/11 17:35)**: "Autorizo retomar exclusivamente a F3-T10/T11"; "avançar sem aguardar a matriz B3-RACI-01"; "Execute em etapas testáveis"; "Não conclua T10/T11 sem os testes previstos e minha homologação".
- **B3-RACI-01 NÃO bloqueia a T10/T11** — a matriz é exigida na F3-T02 e na F3-T05/T06.
- **O código do app NÃO está no GitHub** — vive no projeto Skip 51414, versionado por apply (hoje v0.0.160).
- **O endereço de PREVIEW é o que funciona**: `repositorio-adapta-cc556--preview.goskip.app`. O de produção nunca foi publicado.

## F3-T10 — CONCLUÍDA (11/11/2026)

**Cinco etapas, cinco homologações:** E1 colunas e contagens → E2 cartão completo → E3 filtros → E4 Jornada → E5 arrastar com evidência.

**A prova do CA-3-14 é do próprio owner.** Ele arrastou "Serviços Delta Ltda." (`PIPE-F3-IN-001`) de Oportunidade para Proposta, com evidência. Conferido: `estado = proposta`, evento `oportunidade → proposta` às 15:11:30 de 11/11.

**Quatro defeitos reais corrigidos:** (1) zero falso sem sessão; (2) contador fora do filtro; (3) hora crua em UTC; (4) tela em branco por ordem de hooks.

**Nenhuma coleção, campo, índice ou migration de estrutura nesta task.**

**Ocorrências preservadas:** eventos de teste no histórico em `06_notas/registro-eventos-teste-e5-2026-11-05.md`.

## SEGURANÇA — correção em 6 etapas (autorizada 11/11/2026 12:51)

**Ordem determinada pelo owner:** (1) bloquear escrita anônima em demandas preservando leitura → (2) bloquear cadastro público e impedir autoatribuição de papel → (3) login nas telas Inicial, Funil e Pipeline → (4) conta administrativa real → (5) fechar leitura pública de demandas → (6) bloquear exclusão pública em `decisoes_f2`.

**Backup:** `work/seguranca/backup-regras-ANTES.json` e `backup-regras-ETAPA2.json`. Cada migration tem função de reversão.

### Etapa 1 — APLICADA (v0.0.159) e HOMOLOGADA (11/11 16:21)

| Regra (demandas) | Antes | Depois |
|---|---|---|
| `createRule` | `""` | `@request.auth.id != ''` |
| `updateRule` | `""` | `@request.auth.id != ''` |
| `deleteRule` | `""` | `null` (ninguém apaga pela API) |
| `listRule` / `viewRule` | `""` | **preservados** |

### Etapa 2 — APLICADA (v0.0.160, ref af1622a), NÃO homologada

| Regra (users) | Antes | Depois |
|---|---|---|
| `createRule` | `""` (livre) | `null` — ninguém cria conta pela API |
| `updateRule` | `id = @request.auth.id` | `id = @request.auth.id && @request.body.role:isset = false` |
| `listRule` / `viewRule` / `deleteRule` | `id = @request.auth.id` | **preservados** |

**Testes:** T1 login funciona; T2 criar conta sem login RECUSADO (403); T3 criar logado RECUSADO (403); T5 `role` no pedido RECUSADO (404); T6 alterar o próprio nome ACEITO; T7 conta intacta.

### Diagnóstico: Funil travado (16:10) — resolvido

Owner relatou "Carregando..." e "Something went wrong" às 16:02. Erro **não reproduzido** em 6 cenários; 7 hipóteses descartadas com prova. Causas: instabilidade do Skip às 15:5x e ausência de limite de espera na tela. Owner confirmou às 16:21 que o Funil abre no preview. Documento: `06_notas/diagnostico-funil-carregando-2026-11-11.md`. Melhoria pendente (não aplicada): limite de espera e mensagem clara na tela do Funil.

### Diagnóstico: páginas em branco (17:20) — nada alterado

**Causa comprovada:** o endereço de **produção** foi acessado. O app nunca foi publicado (`isPublished: false`), e a página de produção do Skip mostra "Produção não publicada — Esse app ainda não foi publicado". O elemento `root` nessa URL tem **0 bytes**: não há aplicativo servido ali, daí o branco sem mensagem.

| URL | Estado |
|---|---|
| `repositorio-adapta-cc556--preview.goskip.app` | **funciona** — todas as homologações foram feitas aqui |
| `repositorio-adapta-cc556.goskip.app` | **nunca publicado** — só o aviso do Skip |

**Evidências no preview (sessão limpa, storage apagado):** `/funil` root 101.964 B com 13 cartões; `/` root 61.478 B com painel; `/pipeline-t01` root 20.435 B. Arquivos: as 4 rotas devolvem 200 com HTML de 1.268 B apontando para `assets/index-DQVPeI_P.js`, que responde 200 com 2.028.699 bytes.

**A v0.0.160 não introduziu o problema:** `lastDevBuildRef: af1622a` é o build dela, e ele renderiza. A etapa 2 mexeu só em regra de acesso, nenhum arquivo de tela.

**Dados e regras intactos:** health 200 em 35 ms; login funciona; demandas 13, propostas 2, vagas 2, pipeline_log 9, campanhas_meta 200, experimentos_f2 3, decisoes_f2 6; 1 conta `champion`. Proteções ativas: escrita anônima em demandas 404, criação anônima de conta 403, `role` no pedido 404.

**Correção necessária:** nenhuma no preview. A ação é usar o endereço com `--preview`. Publicar em produção fica para depois da etapa 5.

**Pendente do owner:** confirmar qual endereço abriu. Se produção, o diagnóstico fecha e a etapa 2 pode ser homologada.

**Pendências registradas:** (a) auditoria — o histórico grava o `responsavel` da demanda, não quem executou; (b) criar contas reais para Comercial, Operação e Diretoria; (c) varrer todas as rotas de backend antes da etapa 5; (d) limite de espera e mensagem clara na tela do Funil.

**Etapas 3 a 6:** não iniciadas.
