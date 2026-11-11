# Diagnóstico — páginas do ETHOS em branco (11/11/2026, 17:20)

**Relato do owner (17:16):** as páginas do ETHOS no Preview aparecem completamente em branco, sem conteúdo e sem mensagem de erro.
**Estado:** diagnóstico apenas. **Nada foi alterado.** Etapa 2 permanece pendente.

---

## Causa comprovada

**O owner acessou o endereço de PRODUÇÃO, onde o app nunca foi publicado.**

O endereço de produção `repositorio-adapta-cc556.goskip.app` responde com uma página do próprio Skip, não com o aplicativo:

| O que a página diz, literalmente |
|---|
| "Produção não publicada" |
| "Esse app ainda não foi publicado." |
| "A URL de produção só fica disponível após clicar em **Publicar** no Builder." |
| "Projeto: repositorio-adapta" |

**Evidência técnica:** nessa URL, o elemento `root` — onde o React monta a aplicação — tem **0 bytes**. Não há conteúdo porque o aplicativo não é servido ali. É exatamente o sintoma descrito: branco, sem conteúdo, sem mensagem de erro (a única mensagem é a do Skip, em letras pequenas).

**Estado do projeto confirma:** `isPublished: false` e `lastPublishedRef` inexistente. O projeto nunca foi publicado. O owner me proibiu publicar em produção, e eu não publiquei.

---

## As duas URLs — a diferença que causa a confusão

| URL | O que é | Estado |
|---|---|---|
| `repositorio-adapta-cc556**--preview**.goskip.app` | Ambiente de teste | **funciona** — é onde todas as homologações foram feitas |
| `repositorio-adapta-cc556.goskip.app` | Ambiente de produção | **nunca publicado** — mostra só o aviso do Skip |

As homologações da F3-T10 e das Etapas 1 e 2 foram todas feitas no **preview**. Todos os links que eu enviei usavam `--preview`. O owner aprovou a Etapa 1 confirmando que "o Funil do Preview abre normalmente" — a palavra dele próprio foi "Preview".

**Provável origem da confusão:** os dois endereços diferem por 9 caracteres (`--preview`) no meio. Num histórico de navegador ou numa lista de links salvos, um aparece do lado do outro.

---

## O que eu testei e comprovei (evidências)

### 1. Preview, sessão limpa — FUNCIONA

| Rota | HTTP | Conteúdo renderizado |
|---|---|---|
| `/funil` | 200 | 4.446 chars de texto, `root` com **101.964 bytes**, 13 cartões |
| `/` | 200 | painel visível, `root` com **61.478 bytes** |
| `/pipeline-t01` | 200 | `root` com **20.435 bytes** (sem dados, por não estar logado) |

Sessão limpa de verdade: `localStorage` e `sessionStorage` apagados antes do teste.

### 2. Produção — NÃO funciona (e não deveria)

`/funil` no endereço de produção: título "Skip - Página não encontrada", `root` com **0 bytes**.

### 3. Arquivos do aplicativo carregam corretamente

| Rota | HTML | Bundle referenciado |
|---|---|---|
| `/` | 200, 1.268 B | `assets/index-DQVPeI_P.js` |
| `/funil` | 200, 1.268 B | idem |
| `/pipeline-t01` | 200, 1.268 B | idem |
| `/campanhas` | 200, 1.268 B | idem |

O bundle responde **200 com 2.028.699 bytes** — íntegro, é o build da v0.0.160.

### 4. A v0.0.160 não introduziu o problema

`lastDevBuildRef: af1622a` = v0.0.160. O bundle servido é o dela e renderiza normalmente. **A Etapa 2 fechou o cadastro de contas e o campo `role` — não toca em frontend.** Nenhum arquivo de tela foi alterado nesta etapa.

### 5. Dados e regras intactos

| Verificação | Resultado |
|---|---|
| Health do backend | 200 em 35 ms |
| Login da conta sintética | **funciona** |
| demandas / propostas / vagas | 13 / 2 / 2 |
| pipeline_log | 9 |
| campanhas_meta | 200 |
| experimentos_f2 / decisoes_f2 | 3 / 6 |
| Contas | **1**, papel `champion` |
| Demandas: escrita anônima | **404 recusado** (proteção ativa) |
| Contas: criação anônima | **403 recusado** (proteção ativa) |
| Contas: `role` no pedido | **404 recusado** (proteção ativa) |
| Demandas: leitura anônima | 13 registros (aberta de propósito até a etapa 5) |

---

## Correção necessária

### Nenhuma — para o preview

O aplicativo no preview está funcionando. Não há defeito a corrigir na v0.0.160.

### A ação é de endereço, e é sua

**Use sempre o endereço com `--preview`:**

```
https://repositorio-adapta-cc556--preview.goskip.app/funil
```

### Se quiser a produção funcionando

Aí sim existe uma ação: **publicar** o projeto. Mas o owner proibiu expressamente ("não publique o sistema em produção"), e a razão está certa — publicaria um sistema com leitura de demandas ainda aberta. **Recomendação: só publicar depois da Etapa 5**, quando a leitura estiver fechada e houver contas reais.

---

## Melhoria proposta, se o owner quiser (não é correção de defeito)

O endereço de produção hoje mostra o aviso do Skip em letras pequenas, e quem chega lá pensa que o sistema quebrou. Duas opções, ambas fora do escopo atual:

1. **Publicar** depois da Etapa 5 — aí o endereço funciona de verdade.
2. **Não publicar nunca** e trabalhar só no preview — nesse caso, usar um atalho salvo com o endereço certo, para não haver confusão.

---

## O que falta do owner

1. **Confirma que abriu o endereço sem `--preview`?** Se sim, este diagnóstico está fechado e a Etapa 2 pode ser homologada.
2. **Se abriu o endereço COM `--preview` e viu branco**, então o problema é outro e é necessário o print com a barra de endereço — porque no teste do Maestro, na mesma URL, a tela carrega com 13 cartões.

A evidência técnica está toda acima; a única coisa que não é visível do lado do Maestro é a barra de endereço da tela do owner.
