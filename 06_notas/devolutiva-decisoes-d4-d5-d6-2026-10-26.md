# Devolutiva formal ao consultor — decisões de negócio D4/D5/D6 + incorporação D1–D6 · SPEC F3-001

**Data:** 2026-10-26 · **De:** Champion/owner (Luiz Carlos Manni) via Maestro · **Para:** Ramon Pádua (consultor técnico da Adapta) · **Ciência:** Izabel (gestão do projeto) · João Paulo (Champion)

## Contexto

O Champion conversou diretamente com o consultor Ramon Pádua sobre as pendências D4/D5/D6 do pacote de harmonizações da F3-T01 (06_notas/f3-t01-pacote-harmonizacoes-2026-10-08.md, commit d691404). O consultor apresentou propostas e o Champion **valida as três como decisões de negócio da Talent Group**, registradas no changelog em 26/10/2026. Esta devolutiva formaliza as decisões e solicita a incorporação às especificações.

## Decisões validadas pelo Champion (26/10/2026)

### D4 — Persistência do "estado do retorno" (SPEC F3-001, contrato Operação → retorno, linha 43)

**Decisão:** a situação atual da vaga fica armazenada **na própria vaga** (campo da coleção), e cada alteração é registrada **também no histórico**, com rastreabilidade.

**Resolve:** a dúvida "campo da coleção × atributo do log" — os dois mecanismos convivem: estado corrente na vaga, trilha de eventos no log append-only ("novo evento = novo registro"), sem reescrever histórico.

**Destrava:** a etapa E2-coleções da F3-T01 (criação de `propostas`/`vagas` com o campo de estado) e, em cascata, as 8 tasks dependentes (T10/T11 Funil, T03/T04 Gargalos, T05/T06 Handoff, T07/T08 DHO, T02).

### D5 — Encerramento do ciclo de substituição (estado do retorno)

**Decisão:** criar a situação **"Substituição encerrada"** na lista fechada versionada do estado do retorno, com **motivo obrigatório** em lista fechada de 2 valores: **"reposto"** ou **"encerrada sem reposição"**. O encerramento da substituição **não encerra automaticamente a vaga**.

**Resolve:** a opção (a) da dúvida de 08/10 (valor adicional na lista fechada versionada) e **supersede** o ponto aberto registrado na DECISÃO de 08/10 ("'Substituição encerrada' NÃO aprovado — permanece dúvida").

**Destrava:** a etapa E5 da F3-T01 (retorno/escalonamento).

### D6 — Regras de recuperação comercial (RN-F3-004)

**Decisão:** mantidas integralmente as regras decididas pelo Champion em 24/09/2026: no máximo **3 tentativas** de recuperação em até **90 dias corridos**; esgotadas as condições, o sistema **sugere** "sem timing", mas a mudança depende de **confirmação expressa do Comercial**; **nenhuma alteração automática**.

**Resolve:** a incorporação à RN-F3-004 (ou regra correlata da SPEC F3-001), preservando a taxonomia F1 (sem timing/perdido como estados terminais com motivo) e sem fluxo paralelo de responsáveis.

**Destrava:** a etapa E6 da F3-T01 (recuperação de oportunidade parada).

## Solicitação ao consultor

1. **Emenda datada à SPEC F3-001** incorporando D4, D5 e D6 conforme acima (D6 também na RN-F3-004), em lote único.
2. **Na mesma emenda, incorporar D1–D3** — decisões do Champion de 07–08/10/2026 que também seguem sem emenda formal:
   - **D1 (vocabulário):** `oferta_servico` mantém exclusivamente R&S / TMO / R&S + TMO; "R&S/Alocação/SOS" é descrição da operação que devolve o retorno, não valor de campo; SOS = Serviço Orientado a Substituições, dentro do TMO.
   - **D2 (área):** área = departamento funcional do cargo da vaga; lista fechada v1 com 13 áreas (TI, Comercial/Vendas, Logística/Supply Chain, Projetos/PMO, Engenharia/Manutenção, Financeiro/Contábil, Administrativo, Marketing, Recursos Humanos, Operações/Produção, Educacional/Acadêmico, Auditoria/Riscos, Laboratorial/Saúde); "Não informado" = condição provisória, não área definitiva.
   - **D3 (RN-F3-002 × "Não informado"):** área "Não informado" não bloqueia o avanço e não gera pendência automática nova; confirmação humana responsável, sem inferência.
3. Após a publicação das emendas: conferência documental pelo Maestro (repo × SPEC), atualização dos defaults conservadores e levantamento das travas E2/E5/E6 — a F3-T01 retorna ao fluxo normal (autorização de execução, provas da SPEC, teste humano).

**Prazo sugerido:** mantido o 31/10/2026 sugerido pelo Champion na cobrança de 24/10 (sugestão, sem trava).

## Situação do pacote D1–D6 (verificação documental de 26/10)

| Item | Decisão do Champion | Emenda formal na SPEC |
|---|---|---|
| D1 vocabulário R&S/Alocação/SOS | 07/10/2026 | **Não** |
| D2 área + lista fechada v1 | 07–08/10/2026 | **Não** |
| D3 RN-F3-002 × "Não informado" | 08/10/2026 | **Não** |
| D4 persistência do estado do retorno | **26/10/2026** (validação de proposta do consultor) | **Não** |
| D5 encerramento da substituição | **26/10/2026** (validação de proposta do consultor) | **Não** |
| D6 regras de recuperação (RN-F3-004) | **26/10/2026** (validação de proposta do consultor; regras de 24/09 mantidas) | **Não** |

**Conclusão:** as 6 pendências têm decisão de negócio do Champion; **nenhuma tem emenda datada na SPEC** — a emenda única D1–D6 é a última trava documental da F3-T01.

## Enquanto a emenda não chega

- F3-T01 permanece **bloqueada** (ordem do Champion de 26/10), sem falha atribuída ao time.
- A Fase 3 avança pelas tasks não bloqueadas: **F3-T12** (Campanhas Meta — plano v2 pronto, aguarda autorização específica de execução; não depende da T01) e, na sequência, F3-T13.
- Nenhuma alteração técnica nesta etapa (ordem do Champion).

---
*Devolutiva preparada pelo Maestro em 26/10/2026, com autorização documental do Champion (somente controle documental). Registro correspondente no changelog em 26/10/2026.*