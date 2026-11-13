# Correção do botão Revogar — HOMOLOGADA

**Versão:** v0.0.192 (ref e307e27)
**Homologada por:** Luiz Carlos Manni, em 13/11/2026
**Status: HOMOLOGADA — não alterar esta funcionalidade daqui em diante.**

---

## O que o owner confirmou pessoalmente

| # | Verificação | Resultado |
|---|---|---|
| 1 | O formulário de revogação abre ao clicar em Revogar | OK |
| 2 | A rolagem automática até o painel funciona | OK |
| 3 | O cursor é posicionado no campo de justificativa | OK |
| 4 | Cancelar fecha o painel sem revogar nada | OK — nenhuma decisão revogada |

---

## O defeito corrigido

O painel de justificativa da revogação nascia no FIM do componente, depois da lista inteira de decisões, SEM rolagem automática até ele. Quem clicava em "Revogar" não via nada acontecer: sem erro, sem mensagem, sem mudança visível.

**Prova da assimetria:** o formulário de registro tinha `scrollIntoView`; o de revogação não tinha — mesmo arquivo, tratamentos diferentes.

---

## A correção aplicada

Arquivo único: `src/components/f2/DecisaoMarketingSection.tsx`

| # | Mudança |
|---|---|
| 1 | `revokeRef` e `motivoRef` — refs para o painel e para o campo |
| 2 | `useEffect` que, ao abrir o painel, rola até ele e põe foco no campo |
| 3 | Texto de orientação: "Preencha o motivo abaixo... a decisão não é apagada" |
| 4 | Aviso "A justificativa é obrigatória para confirmar" quando o campo está vazio |
| 5 | `title` no botão desabilitado explicando o que falta |

**Escopo:** 42 linhas em 8 blocos, todas dentro do painel de revogação e do bloco de refs. Conferido que `load`, `revogarDecisao`, `criarDecisaoPendente` e `registrarDecisao` ficaram intactos.

---

## Backup preservado

| Item | Valor |
|---|---|
| Arquivo | work/seguranca/backup-revogar/DecisaoMarketingSection.ORIGINAL.tsx |
| Bytes | 17.819 |
| Linhas | 487 |
| SHA-256 | 0585e9dc675c113c418d8ce5e5c60653... |
| Versão anterior | v0.0.191 (ref 82fb332) |
| Bundle da correção | work/seguranca/backup-revogar/bundle-v0192.js |

---

## Registro de erros do processo

Antes de chegar à causa, o Maestro afirmou DUAS vezes que o botão funcionava, sem nunca ter clicado nele:

1. Primeiro disse que "faltava o passo do motivo" — verdade parcial que escondia a causa real.
2. Depois disse que o código estava íntegro e que o status poderia ser outro — o print do owner mostrou que o status era exatamente o certo.

**Lição, a terceira vez no mesmo mês:** ler código não é testar. Código correto não prova tela funcionando. Quando não houver sessão válida para clicar, dizer isso em vez de concluir.
