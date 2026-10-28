# Aceite escrito — B3-META-LEITOR (alternativa B da SPEC F3-006)

**Data:** 2026-10-28 09:46 · **Quem aceita:** João Paulo Oliveira — Champion do projeto Adapta (declarado pelo próprio no chat) · **Registro de origem:** respostas ao formulário de 28/10/2026

## Por que este documento existe

A SPEC F3-006 prevê duas formas de fechar a trava B3-META-LEITOR antes da F3-T12: (a) o leitor dedicado somente leitura conclui o Connect Link, **ou** (b) **o Champion registra por escrito que aceita usar o principal atual com instrução estrita de somente leitura**. O Champion escolheu a forma (b) em 28/10/2026. Este documento é o registro escrito que a SPEC exige.

## O que fica aceito

1. **Mecanismo:** a automação da F3-T12 usa o **OAuth do conector Meta Ads do Maestro**, com login por consentimento — exatamente o mecanismo aprovado na SPEC. O passe dura cerca de 60 dias e é renovado pelo consentimento, conforme a política de credenciais do plano (06_notas/plano-f3-t12-campanhas-meta-2026-10-24.md §12).
2. **Acesso:** o acesso usado é o do **principal conectado nesse conector**, com **instrução estrita de SOMENTE LEITURA**.
3. **O que a automação pode fazer:** ler as métricas de campanha da conta `act_1667348577717128` (TalentGroup_01), limitadas ao contrato de campos da SPEC: identificador e nome da campanha, status, objetivo, orçamento diário, início/fim, gasto, impressões, cliques, resultados e data de referência.
4. **O que a automação nunca faz:** ativar, pausar, criar ou editar campanha, conjunto ou anúncio; alterar orçamento ou criativo; ler formulários de lead ou qualquer dado pessoal de contato; escrever em qualquer conta; enviar dados a outro projeto que não o "repositório adapta".
5. **Falha e renovação:** passe expirado ou erro de leitura → faixa de aviso na tela + e-mail ao Champion; o último dado bom é preservado e a próxima execução tenta novamente, sem repetição em sequência.
6. **Reversão:** revogar a conexão do conector e desligar a automação; as coleções permanecem como histórico; nada é apagado.

## Limites deste aceite

- **Vale exclusivamente para a F3-T12 e a F3-T13** — leitura de métricas de campanha. Qualquer outro uso exige novo aceite escrito.
- **Não dispensa** as regras da SPEC: RN-F3-025 (somente leitura na Meta), RN-F3-026 (destino único), RN-F3-027 (último dado bom preservado), RN-F3-028 (sem dado pessoal) e RN-F3-029 (automação com ficha e task).
- **Não altera** a política de LGPD (B3-LGPD-01) para envios de formulário da landing page — esses seguem fora do escopo.
- **Pendência operacional:** o principal exato que a automação usará deve ser conferido na tela do conector antes da primeira execução. Se não for o declarante deste aceite, o aceite precisa ser reafirmado pelo titular da conexão (ou a conexão trocada). Registro atual: a SPEC referencia o principal João Paulo Oliveira; a conexão do Hub usa manni@talentgroup.com.br — são conexões distintas e a do Maestro precisa ser confirmada.

## Decisões correlatas — respostas de 28/10/2026

| Pergunta | Resposta do Champion | Efeito prático |
|---|---|---|
| Como o programa entra na Meta | Manter o login com consentimento (design aprovado) | Não muda o que foi aprovado; renovação a cada ~60 dias pelo consentimento |
| Usuário de sistema Adapta_TG1 | Foi criado para o Adapta — pode usar só leitura | **Aval registrado**, mas **não usado nesta etapa**: o mecanismo escolhido é o login por consentimento. **A permissão dele não será alterada agora** — não se sabe quem mais o utiliza, e mexer nisso é risco desnecessário. Fica como opção documentada para o futuro (token sem expiração), se o consultor concordar |
| Conversions API System User | — | Fora do escopo da F3-T12 (integração de conversões, não de leitura de relatórios) |
| Quem será o leitor | Aceitar por escrito o acesso mais amplo (alternativa b) | Fecha o B3-META-LEITOR sem criar usuário na Meta |
| Procedimento sem tocar no Hub | Confirmado: só adicionar ou verificar, sem alterar o que existe | Hub 57113 permanece ativo e intocado durante todo o desenvolvimento e os testes |

---
*Aceite registrado pelo Maestro em 28/10/2026, com base nas respostas do Champion no chat. Nenhuma alteração executada na Meta, no Skip ou no Hub. A implementação da F3-T12 depende de autorização expressa do Champion em mensagem separada.*