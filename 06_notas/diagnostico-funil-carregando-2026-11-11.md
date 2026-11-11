# Diagnóstico — Funil travado em "Carregando" para o owner (11/11/2026)

**Relato do owner (16:02):** acessou o Funil e ficou indefinidamente em "Carregando...", depois exibiu "Não foi possível carregar o Funil — Something went wrong". Isso contradiz a validação que eu apresentei.
**Estado:** diagnóstico apenas. **Nada foi alterado.** Etapa 1 permanece pendente de homologação.

---

## O que eu tentei reproduzir (e não consegui)

| # | Cenário | Resultado |
|---|---|---|
| 1 | Abrir `/funil` sem sessão, aba nova | carrega — "mostrando 13 de 13", 13 cartões |
| 2 | Limpar sessão e recarregar | carrega normalmente |
| 3 | Sessão inválida/expirada injetada e recarregar | carrega, com o aviso "propostas/vagas não puderam ser lidas" |
| 4 | Consultar as 3 coleções em paralelo, 5 rodadas | 200 em todas as 15 chamadas |
| 5 | Health check, 10 chamadas seguidas | 200 em todas, 21–97 ms |
| 6 | Consultar `demandas` do próprio navegador, 4 vezes | 200 em todas |

**Não reproduzi o erro.** Isso significa que não posso afirmar a causa com a certeza que afirmo outras coisas — e a honestidade aqui vale mais que uma explicação bonita.

---

## O que eu consegui descartar, com prova

| Hipótese | Como testei | Veredito |
|---|---|---|
| A proteção da v0.0.159 quebrou a leitura | `GET demandas` sem token → **200, 13 registros** | **descartada** — a leitura continua aberta |
| A escrita ficou aberta por engano | `PATCH` sem token → **404 recusado** | **descartada** — a proteção está ativa |
| Problema no servidor (banco fora) | 10 health checks + 15 chamadas paralelas | **descartada** agora; **houve instabilidade às 15:5x** (o próprio MCP respondeu "shard indisponível") |
| Auto-cancelamento do PocketBase | `client.ts` tem `autoCancellation(false)` | **descartada** |
| Realtime falhando sem sessão | o Funil **não usa** realtime | **descartada** |
| Build desatualizado no navegador | bundle servido é `index-DQVPeI_P.js`, o da v0.0.159 | **descartada** (no meu navegador) |
| Dados corrompidos | 13 registros, 9/1/2/1, todos sintéticos, 9 eventos | **descartada** |

---

## As duas causas que continuam possíveis

### Causa A — instabilidade do servidor do Skip (mais provável)

Às 15:5x, a própria ferramenta de administração do Skip respondeu: **"shard da app repositorio-adapta-cc556 indisponível agora (banco lento ou fora)"**. Minutos depois, tudo respondia em 20–40 ms.

O comportamento descrito pelo owner — **"fica indefinidamente em Carregando" e depois cai com erro** — é exatamente o que acontece quando a chamada ao banco não retorna. Se a lentidão bateu no momento em que ele abriu a tela, o resultado é esse.

**Por que minha validação não pegou:** eu testei às 13:05 e ele às 16:02. Entre os dois, houve a janela de indisponibilidade. **Validação em um momento não cobre outro** — e eu apresentei "funcionando" como se fosse uma propriedade, quando era uma foto.

### Causa B — a tela não tem limite de espera nem mensagem útil

Independentemente da causa A, há um defeito real e meu:

1. **Não há limite de tempo.** A tela espera para sempre. Se o banco demorar, ela fica em "Carregando" sem prazo.
2. **O erro que aparece é o do banco, cru.** "Something went wrong" é a mensagem padrão do PocketBase quando a requisição é abortada — ela não diz nada a quem lê. A tela não traduz, não explica, não oferece "tentar de novo".
3. **A mensagem aparece em inglês**, porque o texto do erro vem da biblioteca e é exibido sem tratamento.

---

## Correção mínima proposta (sem desfazer a proteção)

Nenhuma linha toca nas regras de acesso. A proteção da v0.0.159 fica como está.

| # | Correção | Onde | O que resolve |
|---|---|---|---|
| 1 | **Limite de espera de 15 segundos** na leitura do Funil; ao estourar, mensagem clara em português e botão "Tentar de novo" | `src/pages/FunilF3Page.tsx` e `src/services/funil.ts` | acaba o "Carregando" infinito |
| 2 | **Traduzir o erro** em vez de exibir o texto cru do banco | idem | "Something went wrong" vira "O servidor não respondeu. Tente de novo em instantes." |
| 3 | **Repetir a consulta automaticamente uma vez** antes de mostrar erro | idem | resolve a maioria dos casos de instabilidade momentânea |

**Impacto:** só a tela do Funil. Nenhuma regra, nenhum dado, nenhuma outra tela.

**Risco:** baixo. Se a correção falhar, a tela volta a se comportar como hoje — não há como piorar o acesso ao banco.

---

## O que eu preciso de você para fechar o diagnóstico

Não consigo reproduzir no meu navegador, então preciso de um dado seu:

1. **A qual endereço você acessou?** `repositorio-adapta-cc556--preview.goskip.app/funil` ou o endereço de produção `repositorio-adapta-cc556.goskip.app/funil`? São builds separados — o de produção **nunca foi publicado** (`isPublished: false`), então pode estar servindo uma versão antiga.
2. **O erro acontece sempre ou só às vezes?** Se sempre, é build/configuração. Se às vezes, é instabilidade.
3. **Um print da tela** com a barra de endereço visível resolve as duas perguntas de uma vez.

**Sem isso, eu estaria adivinhando** — e eu já errei uma vez hoje por não conferir antes de afirmar.

---

## Sobre a minha validação anterior

Ela estava errada em método, não em má-fé: eu testei **uma vez**, no meu navegador, sem sessão, e concluí "funciona". Deveria ter dito: *"funcionou agora, às 13:05, no meu ambiente"* — que é diferente de "está funcionando". A frase que usei deu ao owner uma garantia que eu não tinha como dar.

---

## Confirmações pedidas pelo owner

**3. Os 13 registros continuam intactos?** Sim. Conferido: 13 registros, estados 9 suspect / 1 prospect / 1 proposta / 2 oportunidade, todos `origem_dado=sintetico`, 9 eventos no histórico. Nada foi criado, alterado ou apagado.

**4. A v0.0.159 provocou o problema?** Pelo que consegui testar, **não**: a leitura anônima continua respondendo 200 com 13 registros, e a escrita anônima continua recusada. A migration 0039 só alterou `createRule`, `updateRule` e `deleteRule` — `listRule` e `viewRule` foram preservados exatamente como estavam. Se o erro fosse causado por ela, a leitura estaria falhando agora, e não está.

**5. Existe outro caminho de acesso público aos mesmos dados?** Verificado parcialmente: as rotas de backend (`/backend/v1/f3-t01/*`, `/backend/v1/campanhas-meta/*`, `/backend/v1/f3-t13/*`) exigem autenticação (`$apis.requireAuth()`). A coleção `demandas` é o único caminho que ainda permite leitura anônima — é justamente o que a etapa 5 vai fechar. **Pendente:** varrer todas as rotas registradas nos hooks e confirmar uma a uma antes da etapa 5.
