# Etapa 4 — contas reais e remoção das credenciais (concluída 12/11/2026)

**Autorizada:** 11/11/2026 19:39 · **Concluída:** 12/11/2026 · **Versão final:** v0.0.181

---

## 1. Contas reais criadas

| Conta | Papel | Nome | Estado |
|---|---|---|---|
| `jpaulo@talentgroup.com.br` | `champion` | João Paulo | ✅ entrou |
| `manni@talentgroup.com.br` | `champion` | Manni | ✅ entrou |

**Como a senha foi definida:** criadas por migration com senha aleatória de 64 caracteres que **nunca foi registrada em lugar nenhum** (nem no arquivo, nem no log, nem no chat). Cada um definiu a própria senha pelo fluxo de redefinição por e-mail. A senha real nunca passou pelo Maestro.

**Por que por migration e não pela API:** a Etapa 2 fechou `createRule` de `users` com `null` — ninguém cria conta pela API, nem logado. A tentativa por API devolveu 403 "Only superusers". A migration roda no contexto interno.

---

## 2. Credenciais removidas do código

| Arquivo | O que saiu | O que entrou |
|---|---|---|
| `src/components/seg/ProtecaoDeRota.tsx` | constante com e-mail e senha | `FormularioDeAcesso` |
| `src/components/seg/AcessoRequerido.tsx` | idem | idem |
| `src/pages/CampanhasF3Page.tsx` | idem | idem |
| `src/pages/PipelineF3T01Page.tsx` | idem | idem |
| `src/pages/ProvaF3T13Page.tsx` | idem | idem |
| `src/pages/ExperimentosF2.tsx` | idem | campos de e-mail e senha no cartão próprio |

Novo componente: `src/components/seg/FormularioDeAcesso.tsx` — formulário único, reutilizado por todas as telas.

---

## 3. Conta de teste desativada

Desativada em **três camadas redundantes**:

1. `verified = false` → login recusado
2. senha aleatória de 64 caracteres → a senha antiga (pública) não vale mais
3. e-mail → `conta-desativada-etapa4@invalid.invalid` → o endereço antigo não encontra conta

**Não apagada**, para preservar o rastro de autoria em eventos do histórico e registros da Fase 2.

---

## 4. Verificações finais

| # | Verificação | Resultado |
|---|---|---|
| 1 | Conta sintética não autentica | ✅ HTTP 400 "Failed to authenticate" |
| 2 | As duas contas champion funcionam | ✅ ambas entraram (confirmado pelo owner) |
| 3 | Senhas expostas no código público | ✅ **0** ocorrências |
| 4 | 13 registros e demais dados intactos | ✅ 13 · 2 · 2 · 9 · 210 |
| 5 | Proteções anteriores ativas | ✅ escrita 404 · apagar 403 · criar conta 403 |

---

## 5. Tela nova: `/reset-password`

Criada porque o sistema enviava o e-mail de redefinição mas **não tinha onde a pessoa clicasse**. `src/pages/RedefinirSenhaPage.tsx`:

- lê o código do link, pede a nova senha duas vezes, grava pela rota de confirmação do PocketBase;
- exige mínimo de 10 caracteres e confere se as duas digitações batem;
- não aparece no menu (é tela de passagem, aberta só pelo link).

O modelo de e-mail `passwordReset` foi corrigido: o link apontava para a **produção**, que nunca foi publicada. Agora aponta para o preview, com o assunto "Redefina sua senha — ETHOS".

---

## 6. O que ficou pendente para a Etapa 5

A leitura pública de `demandas` continua aberta (13 registros para quem não está logado). São dados fictícios, mas é o último acesso anônimo relevante.

---

## 7. Nota sobre um erro do Maestro nesta etapa

Ao consultar a lista de usuários pela API, o sistema devolveu apenas a conta de teste, e o Maestro concluiu que as contas reais não tinham sido criadas. Era falso: a `listRule` de `users` é `id = @request.auth.id`, ou seja, cada usuário vê apenas a si mesmo — proteção aplicada na própria Etapa 2. A criação estava correta desde o início.

**Lição:** ao consultar um sistema que acabou de ser protegido, a própria proteção limita o que se consegue ver. Consulta vazia não prova ausência.
