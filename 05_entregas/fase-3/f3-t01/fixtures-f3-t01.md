# Fixtures F3-T01 — Especificação dos cenários sintéticos (E1)

**Data:** 2026-10-09; consolidado em 29/10/2026 conforme E-F3-001-04 · **Task:** F3-T01 · **Etapa:** E1 (autorizada pelo Champion em 09/10/2026 — início parcial)
**Natureza:** 100% sintética — empresas, pessoas, propostas e vagas fictícias; nenhum dado real, nenhuma integração externa, nenhuma publicação.
**Vocabulários:** exclusivamente os aprovados — `oferta_servico` ∈ {R&S, TMO, R&S + TMO} (default D1); `área` ∈ lista v1 de 13 áreas + "Não informado" provisório (default D2); estado do retorno ∈ {Em andamento, Perfis apresentados, Aguardando retorno do cliente, Substituição em andamento, Substituição encerrada} (DECISÃO 08/10 atualizada pela E-F3-001-04, D5); resultado ∈ {Sem desfecho, Sucesso, Sem sucesso} (DECISÃO 08/10).
**Gate:** `lead qualificado` permanece BLOQUEADO (RN-F1-011/B3-MET-01) — os fixtures NÃO atravessam esse estado; a pendência é visível no card, jamais simulada como aprovada.
**Marco:** travessia `proposta → vaga aberta` exige `status_proposta=aceita` (F1-T04, inalterado); `data_conversao_comercial` autopreenchida e nunca sobrescrita.

---

## PIPE-F3-IN-001 — jornada inbound completa (R&S)

| Campo | Valor |
|---|---|
| record_id | PIPE-F3-IN-001 |
| tipo_origem | inbound |
| canal | site (fictício) |
| empresa | Serviços Delta Ltda. (fictícia) |
| contato | pessoa fictícia (sem dado pessoal real) |
| oferta_servico | R&S |
| estado inicial → final | suspect → prospect → oportunidade → proposta (aceita) → vaga aberta → ganho |
| handoff_status | pendente → aceito (suspect→prospect, conforme F1-T05) |
| icp_validado | sim (validação HUMANA simulada pelo papel GN — fixture) |
| lead_qualificado | NÃO atravessado — gate bloqueado; pendência visível registrada |
| responsável | GN fictício (dono provisório — B3-RACI-01) |
| proposta | nº PROP-IN-001; área = TI; serviço = R&S |
| vaga | nº VAGA-IN-001; área = TI; prazo operacional = data futura fictícia; capacidade = 1 |
| retorno da operação | eventos: Em andamento → Perfis apresentados (data fictícia) → Aguardando retorno do cliente → resultado Sucesso; motivo: contratação realizada (fictícia) |
| evidências por transição | dono, próxima ação, prazo e motivo em cada passo (RN-F1-007/008) |
| terminal | ganho com motivo e evidência de decisão (RN-F1-010 — nova evidência nesta atualização) |

## PIPE-F3-OUT-001 — jornada outbound completa (TMO)

| Campo | Valor |
|---|---|
| record_id | PIPE-F3-OUT-001 |
| tipo_origem | outbound |
| canal | linkedin (fictício) |
| empresa | Logística Ômega S.A. (fictícia) |
| contato | pessoa fictícia |
| oferta_servico | TMO |
| estado inicial → final | suspect → prospect → oportunidade → proposta (aceita) → vaga aberta → ganho |
| handoff_status | pendente → aceito |
| icp_validado | sim (humano, fixture) |
| lead_qualificado | NÃO atravessado — gate bloqueado; pendência visível |
| responsável | GN fictício (dono provisório) |
| proposta | nº PROP-OUT-001; área = Logística/Supply Chain; serviço = TMO |
| vaga | nº VAGA-OUT-001; área = Operações/Produção (área diferente da proposta — demonstra área por registro); prazo operacional = data futura fictícia; capacidade = 3 |
| retorno da operação | eventos: Em andamento → Perfis apresentados → resultado Sucesso; motivo: alocação efetivada (fictícia) |
| cenário de substituição (leitura, sem execução) | após o Sucesso, evento "Substituição em andamento" com motivo fictício de reposição SOS — resultado permanece Sucesso (DECISÃO 08/10); encerramento simulado com "Substituição encerrada", motivo "reposto"; cenário alternativo com "encerrada sem reposição"; nenhum dos dois encerra automaticamente a vaga/demanda (D5 aprovada) |
| terminal | ganho com motivo e evidência |

## PIPE-F3-STUCK-001 — oportunidade parada (escalonamento, leitura)

| Campo | Valor |
|---|---|
| record_id | PIPE-F3-STUCK-001 |
| tipo_origem | inbound |
| empresa | Consultoria Sigma Ltda. (fictícia) |
| oferta_servico | R&S |
| estado | oportunidade — sem resposta no prazo da próxima ação (vencido, data fictícia) |
| cenário previsto | pendência visível com dono provisório (Comercial/GN — B3-RACI-01) + caminho para terminal com motivo, conforme RN-F3-004 VIGENTE |
| recuperação comercial aprovada | RN-F3-004 incorpora D6: até 3 tentativas em até 90 dias corridos; testar o esgotamento de cada limite separadamente; sistema somente sugere sem timing, e a transição exige confirmação expressa do Comercial. E6 sem bloqueio documental, execução ainda pendente |
| terminal previsto | sem timing com motivo (após confirmação do Comercial — sem automação) |

---

## Cobertura dos cenários da SPEC (Resultado observável §1–3)

| Cenário da SPEC | Fixture | Status |
|---|---|---|
| §1 — caso inbound e outbound percorrem a jornada com dono/próxima ação/prazo/motivo | PIPE-F3-IN-001, PIPE-F3-OUT-001 | especificados |
| §2 — proposta/vaga devolve resultado da operação com estado, prazo/capacidade e motivo, origem rastreável | eventos de retorno nos dois fixtures | especificados; E2-coleções/E5 sem bloqueio documental, execução/provas pendentes |
| §3 — oportunidade parada tem prazo de escalonamento, estado e caminho terminal com motivo | PIPE-F3-STUCK-001 | especificado; E6 sem bloqueio documental, execução/provas pendentes |

## Validações executadas nesta etapa (E1 — especificação)

1. **Vocabulários:** todos os valores usados pertencem às listas aprovadas (oferta_servico R&S/TMO; áreas da lista v1; estados do retorno e resultados da DECISÃO 08/10). ✔
2. **Gate lead qualificado:** nenhum fixture atravessa; pendência visível declarada em cada card. ✔
3. **Marco F1-T04:** travessia proposta→vaga exige status_proposta=aceita em ambos os fixtures; data_conversao_comercial autopreenchida. ✔
4. **Sem dado real:** empresas/pessoas/datas 100% fictícias; sem contato real, sem integração, sem publicação. ✔
5. **Limite da consolidação:** apenas decisões já aprovadas na E-F3-001-04; este documento especifica provas, não comprova execução. D4/D5/D6 não são mais perguntas pendentes. Nenhum aceite técnico foi acrescentado.

## Situação vigente após a consolidação (29/10/2026)

As dúvidas D4/D5/D6 foram resolvidas pela E-F3-001-04. O registro E1 original acima continua sendo histórico de especificação; não constitui prova de execução dos cenários novos.

- **D4:** em ambos os fixtures inbound/outbound, manter o estado corrente do retorno na vaga e registrar cada mudança em novo evento append-only com vínculo/autoria/data/evidência. Comprovar o estado após recarregar e comparar a sequência do log; sem reescrever eventos antigos.
- **D5:** no fixture outbound, testar "Substituição encerrada" nos dois motivos aprovados; recusar motivo vazio/inválido e comprovar que vaga e demanda não são encerradas automaticamente.
- **D6:** no fixture STUCK, testar 1ª–3ª tentativas dentro da janela; recusar 4ª tentativa; em cenário separado, esgotar 90 dias com menos de 3 tentativas. Em ambos os esgotamentos, só sugerir "sem timing"; estado só muda com confirmação expressa do Comercial e evidência. Não reiniciar contagem/janela. Se a referência inicial da janela não estiver demonstrada em decisão anterior, validar esse detalhe com o Champion antes de implementar, sem reabrir a regra aprovada.

**Permanecem pendentes:** implementação ainda não comprovada de E2-coleções/E5/E6, autorização antes de executar, provas automatizáveis, evidências e teste humano. F3-T01 não está concluída. Preservar gates de dado real/LGPD, meta, RACI e DHO aplicáveis.
