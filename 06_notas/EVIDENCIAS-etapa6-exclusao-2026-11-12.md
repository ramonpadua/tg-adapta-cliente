# Etapa 6 — Evidências da homologação (12-13/11/2026)

**Objeto:** bloqueio da exclusão definitiva das decisões da Fase 2 (`decisoes_f2`).

**Regra aplicada:** `deleteRule = null` — nenhum usuário apaga pela API, INCLUSIVE champion.

**Versões:** regra em v0.0.186 (migration 0049) · tela de prova em v0.0.193 (temporária, removida depois)

---

## 1. A regra, lida na fonte

| Regra | Valor | Situação |
|---|---|---|
| listRule | exige login | preservada |
| viewRule | exige login | preservada |
| createRule | exige login + status pendente | preservada |
| updateRule | exige champion ou delegado | preservada |
| **deleteRule** | **null** | **a única alterada pela Etapa 6** |

`null` no PocketBase significa operação proibida pela API. Não é "só quem tem papel X" — é ninguém.

---

## 2. Evidência — exclusão ANÔNIMA

Chamada feita contra um identificador INEXISTENTE (`prova_etapa6_id_que_nao_existe`), para que nenhum registro real pudesse ser apagado mesmo se a regra estivesse aberta.

```
DELETE /api/collections/decisoes_f2/records/prova_etapa6_id_que_nao_existe
HTTP 403 - "Only superusers can perform this action."
```

O servidor recusou ANTES de procurar o registro. Não foi "não encontrei" — foi "você não pode".

**Contraprova:** se a regra estivesse aberta (como antes da Etapa 6), a mesma chamada devolveria 404 "não encontrado". A diferença entre 403 e 404 é a prova objetiva.

---

## 3. Evidência — leitura anônima

```
GET /api/collections/decisoes_f2/records?perPage=1
totalItems: 0 · itens: 0
```

O anônimo não lê decisões. Zero registros NÃO significa coleção vazia: os 6 existem e são filtrados pela regra.

---

## 4. Evidência COMPLETA — teste com sessão CHAMPION

**Executado pelo owner**, na tela `/prova-etapa6`, com a própria conta.

**Print guardado:** `06_notas/evidencias/PROVA-etapa6-champion-bloqueado-2026-11-12.png` (105.334 bytes, sha256 dd8e0be9c68b0cbf...)

### O que o print mostra

| Campo | Valor |
|---|---|
| conta | jpaulo@talentgroup.com.br |
| **papel** | **champion** |
| Resultado | **Exclusão BLOQUEADA — a regra está fechada** |
| Detalhe | HTTP 403 · Only superusers — id usado: prova_etapa6_id_que_nao_existe |

### Por que é a prova decisiva

O teste foi feito autenticado como champion — o papel de maior poder do sistema. Mesmo assim o servidor recusou com HTTP 403. Isso demonstra:

1. A regra vale para todos, não só para anônimos;
2. Champion não é exceção — exatamente o que o owner determinou em 12/11 (opção B);
3. A recusa é do SERVIDOR: a tela só exibiu a resposta do banco.

### As duas evidências juntas

| Sessão | Resposta | Conclusão |
|---|---|---|
| Anônimo | 403 Only superusers | bloqueado |
| Champion (jpaulo@) | 403 Only superusers | bloqueado |

---

## 5. Registros preservados

| Verificação | Resultado |
|---|---|
| Decisões apagadas | 0 |
| Decisões alteradas | 0 |
| Decisões revogadas | 0 |
| Identificador usado | prova_etapa6_id_que_nao_existe — inexistente por construção |

---

## 6. ETAPA 6 — HOMOLOGADA

**Homologada pelo owner em 13/11/2026.**

| # | Verificação exigida | Resultado |
|---|---|---|
| 1 | Exclusão bloqueada para anônimos | OK — 403 |
| 2 | Exclusão bloqueada para autenticado | OK — 403 |
| 3 | Exclusão bloqueada para champion | OK — 403 (print) |
| 4 | Nenhum registro apagado ou modificado | OK — 0 alterações |
| 5 | Leitura, criação e alteração preservadas | OK — conferidas na fonte |

**A segurança do ETHOS está concluída nas 6 etapas.**

---

## 7. A tela de prova

`/prova-etapa6` foi TEMPORÁRIA. Cumpriu a função de permitir o teste com sessão champion e foi REMOVIDA do sistema em 13/11/2026, após a homologação. As evidências que ela produziu estão preservadas neste documento e no print acima.
