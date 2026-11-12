# Etapa 5 — leitura pública de demandas fechada (12/11/2026)

**Autorizada:** 12/11/2026 10:51 · **Aplicada:** v0.0.184 · **Aguardando homologação**

---

## 1. O que mudou

A coleção `demandas` aceitava leitura anônima. Fechada.

| Regra | Antes | Depois |
|---|---|---|
| `listRule` | `""` (livre) | `@request.auth.id != ''` |
| `viewRule` | `""` (livre) | `@request.auth.id != ''` |
| `createRule` | `@request.auth.id != ''` | **inalterada** |
| `updateRule` | `@request.auth.id != ''` | **inalterada** |
| `deleteRule` | `null` | **inalterada** |

**Nada além disso foi tocado.** Nenhum campo, índice, tela, hook ou dado.

---

## 2. Risco verificado ANTES de aplicar

A pergunta era: quem depende da leitura anônima de `demandas`? O fechamento quebraria esse consumidor.

| Consumidor | Lê demandas? | Já exige login? | Risco |
|---|---|---|---|
| Tela Inicial | sim | sim (`useAcessoRequerido`) | nenhum |
| Tela Funil | sim | sim (`ProtecaoDeRota`) | nenhum |
| Tela Pipeline (T01) | sim | sim (`ProtecaoDeRota`) | nenhum |
| Tela Atribuição (T04) | sim | sim (`ProtecaoDeRota`) | nenhum |
| Tela Decisões F2 | sim | sim (`ProtecaoDeRota`) | nenhum |
| Tela Campanhas | não | sim | nenhum |
| Tela Experimentos F2 | não | sim | nenhum |
| Tela Prova Meta | não | sim | nenhum |
| **Automação `campanhas_meta_sync`** | **não** | — | **nenhum** (usa só `conexao_meta` e `campanhas_meta`) |
| **Hooks do servidor** (F1/F3) | sim | — | **nenhum** (rodam no contexto interno, fora das regras de coleção) |
| **Rotas de backend** `/backend/v1/f3-t01/*` | sim | sim (`$apis.requireAuth()`) | nenhum |

**Conclusão: nenhum consumidor legítimo dependia da leitura anônima.**

---

## 3. Testes

### Sem autenticação (deve ser bloqueado)

| Teste | Resultado |
|---|---|
| Listar demandas pela API | **0 registros** (filtrado pela regra) |
| Ler um registro específico (`PIPE-F3-STUCK-001`) | **404 — não encontrado** |
| Abrir o Funil no navegador | mostra o formulário de login, **0 cartões** |
| Abrir a Inicial, Pipeline e Campanhas | HTTP 200, pedem login |

### Regras gravadas (conferidas na fonte, não por suposição)

```
listRule   = "@request.auth.id != ''"
viewRule   = "@request.auth.id != ''"
createRule = "@request.auth.id != ''"
updateRule = "@request.auth.id != ''"
deleteRule = null
```

### As outras coleções seguem protegidas

propostas, vagas, pipeline_log, campanhas_meta, experimentos_f2, decisoes_f2, aprovacoes_f2 — todas devolvem **0 registros** ao anônimo (protegidas, não vazias).

---

## 4. O que eu **não** consigo provar sozinho

**Que o usuário logado continua vendo os 13 registros.** Não tenho as senhas das contas reais — e é correta essa separação. A regra `@request.auth.id != ''` é satisfeita por qualquer sessão válida, e o sistema exige login desde a Etapa 3 sem alteração aqui. Mas a prova final é sua: **entre e confirme os 13 cartões.**

---

## 5. Como reverter

`listRule` e `viewRule` voltam para `""`. Estado original em `work/seguranca/backup-etapa5/REGRAS-ANTES.json`; a migration 0046 tem função de reversão.

---

## 6. Estado da segurança

| Etapa | Situação |
|---|---|
| 1 — escrita em demandas | ✅ homologada |
| 2 — cadastro e papel | ✅ homologada |
| 3 — login por rota | ✅ homologada |
| 4 — contas reais e credenciais | ✅ homologada |
| **5 — leitura de demandas** | **aplicada, aguardando homologação** |
| 6 — exclusão em decisões F2 | não iniciada |

Falta a Etapa 6: `decisoes_f2.deleteRule` está `""` (aberta). É a última fresta do mapa de segurança.
