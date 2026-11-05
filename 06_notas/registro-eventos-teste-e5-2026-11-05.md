# Registro formal — dois eventos de teste técnico no histórico (05/11/2026)

**Decisão do owner:** preservar os dois eventos. Não autorizada exclusão por superusuário nem novas tentativas de limpeza.
**Natureza:** eventos reais de um **teste técnico**, executado em 05/11/2026 pelo Maestro durante a preparação da F3-T10 (etapa E5). **Não representam movimentação comercial legítima.**

## 1. Identificação

**Demanda:**
- `record_id`: **PIPE-F3-STUCK-001**
- id interno: `l4ctzbl1qa1l40w`
- empresa: Consultoria Sigma Ltda. (fictícia)
- `origem_dado`: `sintetico`
- created: 2026-11-04 13:33:09.711Z

**Eventos (2), na ordem em que ocorreram:**

| # | id do evento | quando (UTC) | de → para | autor | origem_dado |
|---|---|---|---|---|---|
| 1 | `qdn60rs36n8i801` | 2026-11-05T13:45:59.736Z | oportunidade → proposta | GN Fictício (provisório — B3-RACI-01) | sintetico |
| 2 | `nspvdbuvc3m37oc` | 2026-11-05T13:46:14.020Z | proposta → oportunidade | GN Fictício (provisório — B3-RACI-01) | sintetico |

Ambos com `campo = estado`, `entidade = demanda`, `origem_evento = humano` e `evidencia = "Teste E5 do Funil: proposta registrada no teste de arrastar"`.

> **Por que `origem_evento = humano`:** o hook que grava o histórico marca toda transição como humana (RN-F1-012: nenhuma transição é automática). O teste foi feito por chamada direta à API, autenticado como o usuário sintético do projeto. O campo descreve o tipo de origem do evento, não distingue chamada de tela de chamada de API.

## 2. O que o teste fez e por quê

Antes de desenhar a interface de arrastar (E5), era necessário saber como o sistema responde a uma transição — de que forma a recusa é devolvida e o que acontece quando a transição é aceita. Foram executados três testes:

1. **Recusa controlada** (suspect → prospect sem `handoff_status = aceito` em `FIX-IN-001`): **recusada** com "Nao e possivel avancar para Prospect: handoff_status=aceito e obrigatorio". **Nenhuma alteração gravada.** Nenhum evento gerado.
2. **Transição aceita** (PIPE-F3-STUCK-001: oportunidade → proposta, com evidência): aceita; estado e evidência gravados; **1 evento** no histórico.
3. **Retorno ao estado original** (proposta → oportunidade): aceito; estado restaurado; **1 evento** no histórico.

Os eventos 1 e 2 da tabela são, respectivamente, os testes 2 e 3 acima. O teste 1 não deixou resíduo.

## 3. Estado dos dados após o teste

| Item | Baseline (antes) | Depois das verificações | Situação |
|---|---|---|---|
| 13 demandas | 13 | 13 | intacto |
| Estados | 9 suspect, 1 prospect, 3 oportunidade | 9 suspect, 1 prospect, 3 oportunidade | **idêntico** |
| PIPE-F3-STUCK-001 `estado` | `oportunidade` | `oportunidade` | **restaurado** |
| PIPE-F3-STUCK-001 `evidencia` | vazia | vazia | **restaurada** (migration 0037) |
| Demais campos da demanda | — | conferidos um a um | intactos |
| 2 propostas / 2 vagas | 2 / 2 | 2 / 2 | intacto |
| `pipeline_log` | **2 eventos** | **4 eventos** | **+2 (este registro)** |
| Coleções | 13 | 13 | nenhuma criada |
| Schema (campos, índices, regras) | — | conferido | **inalterado** |

## 4. Contaminação de indicadores oficiais: nenhuma

- O painel oficial (tela inicial) lê **somente** a coleção `demandas` e **filtra `origem_dado = producao`**. Ele **não lê** `pipeline_log`.
- Hoje há **0 registros de produção**: as 13 demandas são sintéticas. O painel mostra zero, e continuaria mostrando zero mesmo que os dois eventos não existissem.
- Os dois eventos têm `origem_dado = sintetico`, ficando fora de qualquer indicador oficial por definição (RN-F3-023).
- A tela Funil mostra o histórico apenas dentro da Jornada de um cartão, com o selo TESTE no cartão — não agrega nem soma eventos em indicador.

## 5. As três tentativas de limpeza (todas frustradas) e o motivo

| Migration | Versão | Resultado |
|---|---|---|
| `0036_f3_t10_limpa_eventos_teste_e5` | 0.0.154 | Rodou como "applied", **não apagou nada** |
| `0037_f3_t10_limpa_residuo_e5_v2` | 0.0.155 | Rodou como "applied", **não apagou nada**; **sim** limpou a `evidencia` de teste da demanda |
| `0038_f3_t10_limpa_residuo_e5_v3` | 0.0.156 | Rodou como "applied", **não apagou nada** |

**Causa raiz:** eu testava a data de criação com `ev.getString('created')`. `created` é campo de **data**, e `getString` em campo de data devolve texto **vazio** neste runtime — logo a comparação nunca casava e a migration terminava reportando sucesso sem ter apagado nada. **Lição:** limpeza que depende de campo de data lido do jeito errado falha em silêncio e ainda reporta sucesso.

**Efeito no schema:** nenhum. As três migrations só faziam leitura (`findRecordsByFilter`), exclusão de registro (`app.delete`) e escrita em `evidencia`. Nenhuma criou ou alterou coleção, campo, índice ou regra de acesso — verificado: as 13 coleções seguem iguais.

**Efeitos residuais das três migrations:**
- 0036: nenhum.
- 0037: **limpou a `evidencia` de teste** que havia ficado na demanda — efeito desejado e conferido (era `"Teste E5 do Funil: ..."`, agora vazia).
- 0038: nenhum.
- As três permanecem no histórico de migrations como "applied" e **não devem ser removidas** — o histórico de migrations também é registro.

## 6. Regra append-only preservada

O histórico continua **não apagável pela API**: tentativa de `DELETE` em um dos eventos devolveu **HTTP 403 — "Only superusers can perform this action."** A coleção `pipeline_log` mantém `deleteRule: null` e `updateRule: null`. A decisão do owner de preservar os eventos é coerente com essa regra: se fosse possível apagar quando incomoda, o histórico deixaria de valer como prova.

## 7. Como estes eventos devem ser lidos daqui em diante

Quem auditar o histórico e encontrar estes dois eventos deve saber, **antes de desconfiar do sistema**, que:

- são de **05/11/2026**, entre 13:45:59 e 13:46:14 UTC (10:45 e 10:46 em Brasília);
- são de **teste técnico**, não de operação comercial;
- a demanda voltou ao estado original, então o par ida-e-volta é consistente com o que aconteceu;
- **os dois eventos são verdadeiros**: houve mesmo uma mudança para `proposta` e um retorno para `oportunidade`. O histórico não mente — apenas registra um fato que não foi trabalho comercial.

## 8. Verificação de integridade deste registro

- Demanda identificada por `record_id` **e** id interno, conferidos na fonte.
- Eventos identificados por id, com data e hora lidas do registro, não de anotação.
- Contagens conferidas na fonte antes e depois: demandas 13, propostas 2, vagas 2, `pipeline_log` 2 → 4.
- Contaminação de indicador: verificada no código (`Index.tsx` lê só `demandas`, filtra `producao`) e no dado (0 registros de produção).
- Regra append-only: verificada por tentativa real de exclusão (403).
