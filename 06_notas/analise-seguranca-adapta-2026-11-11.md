# Análise de segurança — Adapta ETHOS (11/11/2026)

**Pedido do owner:** analisar a falha de acesso identificada, sem modificar banco ou aplicativo.
**Estado:** **análise apenas. Nada foi alterado.** Nenhuma correção foi aplicada.
**Método:** leitura da configuração de acesso das 13 coleções, do código dos hooks e das telas. **Nenhum teste de escrita foi executado para esta análise** — o único teste de escrita do dia foi o que revelou a falha, às 15:37, e está registrado.

---

## 1. Por que uma demanda pôde ser alterada sem login

A coleção `demandas` está com **todas as regras de acesso vazias**:

| Regra | Valor atual | O que significa |
|---|---|---|
| `listRule` | `""` | qualquer pessoa lê a lista, sem login |
| `viewRule` | `""` | qualquer pessoa lê um registro, sem login |
| `createRule` | `""` | **qualquer pessoa cria demanda, sem login** |
| `updateRule` | `""` | **qualquer pessoa altera demanda, sem login** |
| `deleteRule` | `""` | **qualquer pessoa apaga demanda, sem login** |

No PocketBase, regra vazia (`""`) significa **acesso livre**; regra nula (`null`) significa **acesso bloqueado**. A coleção foi criada na Fase 1 com acesso livre — provavelmente para os testes iniciais — e nunca foi fechada.

**Por que os validadores não barraram:** os hooks da F1/F3 (`pipeline_transicoes_update`, `pipeline_log_demanda`) verificam **regras de negócio** — handoff aceito, ICP validado, evidência, motivo em etapa final. Eles **não verificam quem está pedindo**. São coisas diferentes: um valida o *conteúdo*, o outro validaria a *identidade*. A segunda nunca existiu nessa coleção.

---

## 2. Mapa completo das 13 coleções

| Coleção | Ler | Criar | Alterar | Apagar | Situação |
|---|---|---|---|---|---|
| **demandas** | 🔴 aberto | 🔴 aberto | 🔴 aberto | 🔴 aberto | **CRÍTICO** |
| **users** (contas) | 🟢 só a própria | 🔴 **aberto** | 🟢 só a própria | 🟢 só a própria | **ALTO** |
| **decisoes_f2** | 🟢 logado | 🟢 logado (status pendente) | 🟢 só champion/delegado | 🔴 **aberto** | **ALTO** |
| experimentos_f2 | 🟢 logado | 🟡 logado | 🟡 logado | 🟡 logado | Médio |
| experimento_versoes_f2 | 🟢 logado | 🟡 logado | 🟡 logado | 🟡 logado | Médio |
| criativos_f2 | 🟢 logado | 🟡 logado | 🟡 logado | 🟡 logado | Médio |
| aprovacoes_f2 | 🟢 logado | 🟢 papel exigido | 🟢 papel exigido | 🟢 papel exigido | **Bom** |
| bloqueios_f2 | 🟢 logado | 🟢 papel exigido | 🟢 papel exigido | 🟢 papel exigido | **Bom** |
| propostas | 🟢 logado | 🟢 bloqueado | 🟢 bloqueado | 🟢 bloqueado | **Bom** |
| vagas | 🟢 logado | 🟢 bloqueado | 🟢 bloqueado | 🟢 bloqueado | **Bom** |
| pipeline_log (histórico) | 🟢 logado | 🟢 bloqueado | 🟢 bloqueado | 🟢 bloqueado | **Bom** |
| campanhas_meta | 🟢 logado | 🟢 bloqueado | 🟢 bloqueado | 🟢 bloqueado | **Bom** |
| conexao_meta | 🟢 logado | 🟢 bloqueado | 🟢 bloqueado | 🟢 bloqueado | **Bom** |

**Legenda:** 🔴 acesso indevido · 🟡 só exige login, sem papel · 🟢 protegido

---

## 3. Usuários comuns conseguem operações de administrador?

**Sim, por um caminho indireto.** O campo `role` da conta aceita três valores: `operador`, `champion`, `delegado_f2`. As coleções `aprovacoes_f2`, `bloqueios_f2` e a alteração de `decisoes_f2` exigem papel `champion` ou `delegado_f2`.

O problema: **a criação de conta está aberta** (`createRule = ""`). Qualquer pessoa pode criar uma conta na tabela de usuários. Se ela conseguir definir o próprio `role` como `champion` no cadastro, passa a ter os poderes de aprovação. Não verifiquei se o cadastro permite escolher o papel — não faria um teste que grava — mas **a porta está aberta e a fechadura do papel não foi conferida**.

**Agravante:** o cadastro público é o mesmo caminho que um estranho usaria para entrar no sistema sem convite.

---

## 4. As regras são aplicadas no servidor?

**Sim — e isso é a boa notícia.** As regras do PocketBase são avaliadas no servidor, não na tela. A prova é a coleção `pipeline_log`: a tentativa de apagar um evento pela API devolveu **HTTP 403 "Only superusers can perform this action"**, mesmo com sessão válida. Nenhuma tela precisa colaborar para a regra valer.

**Consequência importante para a correção:** fechar a coleção `demandas` no servidor **não pode ser contornado pela tela**. É a correção certa, e é suficiente.

---

## 5. Correção mínima necessária

### 5.1 A correção que resolve o risco crítico (1 linha por regra)

Na coleção **`demandas`**, trocar as regras de escrita:

| Regra | De | Para |
|---|---|---|
| `updateRule` | `""` | `@request.auth.id != ''` |
| `createRule` | `""` | `@request.auth.id != ''` |
| `deleteRule` | `""` | `null` (ninguém apaga pela API) |
| `listRule` / `viewRule` | `""` | **manter aberto** (ver 5.2) |

**Por que a leitura fica aberta:** a tela de campanhas inicial (painel) e o Funil leem `demandas`. Fechar a leitura quebraria as telas homologadas. **Escrever é o que precisa de proteção; ler não expõe o sistema a alteração.** Se o owner quiser fechar a leitura depois, é uma decisão separada, com impacto nas telas.

**Impacto sobre o que está homologado:** nenhum. A tela Funil exige login para mover o cartão (o botão só aparece logado), e o painel lê sem escrever. Nada que funciona hoje deixa de funcionar.

### 5.2 As outras duas correções recomendadas

| Coleção | Regra | De | Para | Por quê |
|---|---|---|---|---|
| **users** | `createRule` | `""` | `null` | fecha o cadastro público; contas passam a ser criadas só pelo administrador |
| **decisoes_f2** | `deleteRule` | `""` | `@request.auth.role = 'champion'` | decisão registrada não pode ser apagada por qualquer um |

**Risco de mexer em `users.createRule`:** se o sistema depende do cadastro público para alguém entrar, essa pessoa para de conseguir se cadastrar. Hoje as contas são sintéticas e criadas por migration — **não há cadastro público em uso**. Mas é o owner quem decide.

### 5.3 O que **não** é correção mínima

- Fechar `listRule` de `demandas` → quebra as telas.
- Trocar `""` por papel específico nas coleções F2 → exige definir papéis nominais (B3-RACI-01), que não estão definidos.
- Criar middleware ou rota nova → desnecessário; a regra do servidor resolve.

---

## 6. Riscos encontrados, em ordem de gravidade

| # | Risco | Gravidade | O que permite hoje |
|---|---|---|---|
| 1 | `demandas` com escrita aberta | **CRÍTICO** | qualquer pessoa com o endereço muda etapa, cria e apaga demanda sem login |
| 2 | `users` com cadastro aberto | **ALTO** | qualquer pessoa cria conta; possível escalada para `champion` |
| 3 | `decisoes_f2` com exclusão aberta | **ALTO** | qualquer pessoa apaga decisão registrada |
| 4 | F2 (experimentos, versões, criativos) só exige login | Médio | usuário logado sem papel mexe em experimento |
| 5 | Nenhuma trilha de "quem mudou" em `demandas` | Médio | a mudança fica no histórico, mas o campo `autor` vem do dado da demanda, não de quem chamou |

**O risco 5 explica por que os eventos de teste ficaram parecidos com eventos reais:** o `autor` do evento é o `responsavel` da demanda, não a pessoa que fez a chamada. Quem olha o histórico não consegue distinguir. Isso não é falha de acesso, é falta de rastreabilidade — e o histórico só registra que houve mudança, não quem a pediu.

---

## 7. Impacto sobre o aplicativo

| Área | Impacto da correção |
|---|---|
| Tela Funil (arrastar) | **nenhum** — hoje já exige login na prática |
| Tela Funil (leitura, Jornada) | **nenhum** — leitura continua aberta |
| Painel inicial | **nenhum** — só lê |
| Tela Campanhas | **nenhum** — coleção própria, já protegida |
| Pipeline T01 (registrar retorno) | **nenhum** — usa rota autenticada |
| Telas da Fase 2 | **nenhum** para leitura; escrita passa a exigir login (já exige hoje) |
| Automação diária da Meta | **nenhum** — usa rota com chave interna, não a coleção |
| Cadastro de usuário pelo app | **para de funcionar** se `users.createRule` for fechada — hoje não é usado |

---

## 8. Plano de testes controlados (para depois da autorização)

Nenhum teste abaixo será executado antes do "pode" do owner.

| # | Teste | Como | O que prova | Grava algo? |
|---|---|---|---|---|
| T1 | Leitura anônima continua | consultar `demandas` sem sessão | as telas seguem funcionando | não |
| T2 | Escrita anônima é recusada | tentar alterar sem sessão, **uma vez** | a falha está fechada | não (deve ser recusado) |
| T3 | Escrita logada funciona | arrastar um cartão de teste, com evidência | não quebramos o Funil | **sim, 1 evento** |
| T4 | Histórico segue não apagável | tentar apagar um evento | append-only intacto | não |
| T5 | Cadastro público fechado | tentar criar conta sem sessão | a porta de entrada fechou | não |
| T6 | Telas homologadas | abrir Funil, Campanhas, Pipeline, Inicial | nada quebrou | não |

**T3 é o único que grava.** Sugiro executá-lo **depois** de T1, T2 e T6 aprovados, e com um cartão escolhido por você. Alternativa: pular T3 e considerar a escrita logada já provada pelo arrasto homologado de 11/11.

---

## 9. Ordem recomendada de execução

1. **Fechar `demandas` para escrita** (risco crítico). Testes T1, T2, T6.
2. **Fechar cadastro de `users`** (risco alto). Teste T5.
3. **Fechar exclusão de `decisoes_f2`** (risco alto). Sem teste que grava.
4. **T4** para confirmar que o histórico continua protegido.
5. **T3** só se você quiser rever a escrita logada.

Cada passo é uma migration pequena e reversível, com verificação de integridade antes e depois.

---

## 10. O que eu preciso de você

1. Autoriza a **correção mínima** (item 5.1) — só `demandas`?
2. Autoriza também **`users.createRule`** e **`decisoes_f2.deleteRule`** (item 5.2)?
3. O teste **T3** (o único que grava um evento) entra ou fica de fora?

Sem essas três respostas eu não toco em nada.
