# PENDÊNCIA CRÍTICA — credenciais expostas no código público

> **Status: ✅ RESOLVIDA EM 12/11/2026 — v0.0.181**
> Registrada em 11/11/2026 19:06 por decisão do owner. Fechada em 12/11/2026 após as duas contas reais entrarem.

---

## Resolução em uma linha

As 6 cópias da senha saíram do código; as telas passaram a pedir e-mail e senha de cada pessoa; a conta de teste foi desativada em três camadas; e a verificação no arquivo público dá **zero** ocorrências.

---

## 1. O que estava exposto

A senha de `humano-sintetico-aprovador-01@f2.invalid` (`F2-Sintetico-2026!`) estava em texto aberto em 6 arquivos do frontend. O navegador baixa o código inteiro — logo, a senha era pública. A credencial tinha papel `champion`.

| # | Arquivo original |
|---|---|
| 1 | `src/pages/CampanhasF3Page.tsx` |
| 2 | `src/pages/PipelineF3T01Page.tsx` |
| 3 | `src/pages/ProvaF3T13Page.tsx` |
| 4 | `src/pages/ExperimentosF2.tsx` |
| 5 | `src/components/seg/AcessoRequerido.tsx` |
| 6 | `src/components/seg/ProtecaoDeRota.tsx` |

Laudo técnico: `artifacts/LAUDO-credenciais-expostas-2026-11-11.md`

---

## 2. O que foi feito

### As três obrigações da pendência

| Obrigação | Situação |
|---|---|
| Remover as senhas do código | ✅ **feito** — 6 de 6 |
| Revogar/desativar a credencial antiga | ✅ **feito** — três camadas |
| Validar a segurança dos acessos | ✅ **feito** — zero ocorrências |

### Como cada tela ficou

Todas passaram a usar `src/components/seg/FormularioDeAcesso.tsx` — campos de e-mail e senha, autenticação individual, mensagem de erro em português sem revelar se o e-mail existe.

### Como a conta de teste foi desativada

| Camada | O que impede |
|---|---|
| `verified = false` | o PocketBase recusa login de conta não verificada |
| senha aleatória de 64 caracteres | a senha antiga (pública) não serve mais |
| e-mail → `conta-desativada-etapa4@invalid.invalid` | o endereço antigo não encontra mais conta |

**Por que não apagar:** eventos do histórico e registros da Fase 2 apontam para esse usuário. Apagar quebraria o rastro de autoria. Desativar preserva o registro e é reversível.

---

## 3. Verificações finais (12/11/2026, v0.0.181)

### A conta de teste não autentica mais

```
POST /auth-with-password  (humano-sintetico-aprovador-01@f2.invalid)
→ HTTP 400 · "Failed to authenticate."
```

### As duas contas reais existem e funcionam

| Conta | Papel | Prova |
|---|---|---|
| `jpaulo@talentgroup.com.br` | champion | entrou (confirmado pelo owner) + e-mail de redefinição aceito |
| `manni@talentgroup.com.br` | champion | entrou (confirmado pelo owner) + e-mail de redefinição aceito |

### Nenhuma senha no código público

Arquivo servido ao navegador: `index-4Q6a9_nX.js`, 2.042.101 bytes.

| Procura | Resultado |
|---|---|
| `F2-Sintetico-2026!` | **0** |
| `F2-Sintetico` (qualquer variante) | **0** |
| `humano-sintetico-aprovador-01` | **0** |
| Token da Meta (`EAA…`) | **0** |

### Os dados continuam intactos

| Coleção | Total |
|---|---|
| demandas | 13 (9 suspect · 1 prospect · 2 oportunidade · 1 proposta) |
| propostas | 2 |
| vagas | 2 |
| pipeline_log | 9 |
| campanhas_meta | 210 (cresce pelo cron) |

### As proteções anteriores continuam ativas

| Proteção | Resposta |
|---|---|
| escrita anônima em demanda | **404** (recusada) |
| apagar demanda anônimo | **403** (recusado) |
| criar conta anônimo | **403** (recusado) |
| conta de teste | **400** (não autentica) |
| leitura anônima de propostas, vagas, histórico, decisões, experimentos, campanhas | **0 registros** (protegidas) |

**Nota:** a leitura anônima de `demandas` continua aberta (13 registros) — é o que a **Etapa 5** vai fechar.

---

## 4. O que ainda depende da Etapa 5

A leitura pública de `demandas` é o último acesso anônimo relevante. Não contém senha nem dado pessoal (as 13 demandas são fictícias), mas mostra o pipeline a quem tiver o link. Fechar isso é a Etapa 5.

---

## 5. Histórico

- **11/11 19:05** — owner pediu a verificação.
- **11/11 19:05** — 6 cópias encontradas; nenhum outro segredo exposto.
- **11/11 19:06** — owner decidiu tratar na Etapa 4; registrada como pendência crítica obrigatória.
- **11/11 22:40** — contas reais criadas (`jpaulo@`, `manni@`, ambas champion).
- **11/11 22:54** — tela `/reset-password` criada; e-mails de redefinição reenviados com link correto.
- **12/11 10:34** — owner confirmou que os dois entraram.
- **12/11** — 6 credenciais removidas; conta de teste desativada; verificação final: **zero ocorrências**.

**Pendência encerrada.**
