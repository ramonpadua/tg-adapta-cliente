# Aceite reafirmado — fonte Graph API direta na F3-T12

**Data:** 2026-10-30 · **Quem reafirma:** Luiz Carlos Manni, owner/decisor do projeto Adapta · **Registro de origem:** resposta ao formulário de 30/10/2026 ("Sim — mesmo principal, mesmo somente-leitura")

## Por que este documento existe

O aceite escrito de 28/10/2026 (`06_notas/aceite-b3-meta-leitor-2026-10-28.md`) fecha o B3-META-LEITOR pela alternativa (b) da SPEC F3-006, mas descreve um mecanismo específico: **"o OAuth do conector Meta Ads do Maestro, com login por consentimento"**.

Em 30/10/2026 o mecanismo foi alterado: a automação `campanhas_meta_sync` passou a ler a Meta pela **Graph API v21.0 direta**, autenticada por token do principal, guardado no segredo `META_ACCESS_TOKEN` do projeto 51414 e lido apenas em runtime. O motivo da troca foi técnico e está registrado: a conexão `adapta-tg` do Composio ficou em `INITIATED` — o consentimento nunca foi concluído, e por isso a leitura real nunca saiu, mantendo o CA-3-16 aberto.

Como o mecanismo mudou, o aceite de 28/10 não cobria mais o que estava em execução. Este documento é a reafirmação exigida.

## O que fica reafirmado

1. **Mecanismo:** a automação da F3-T12 lê a Meta pela **Graph API v21.0 direta** (`/campaigns` e `/insights`), com token do principal. Somente requisições **GET** — não existe neste hook nenhum POST, PUT ou DELETE contra a Meta.
2. **Principal:** **João Paulo Oliveira** (id `28957746507189663`) — o mesmo principal nomeado no aceite de 28/10. Escopo do token: `ads_read` e `ads_management`. Permanece a instrução estrita de **SOMENTE LEITURA**: o escopo de escrita existe no token, mas não é usado pela automação.
3. **Conta lida:** `act_1667348577717128` (TalentGroup_01), sem qualquer outra conta.
4. **Contrato de campos:** idêntico ao da SPEC — identificador e nome da campanha, status efetivo, objetivo, orçamento diário, início/fim, gasto, impressões, cliques, resultados e data de referência.
5. **Renovação:** o token em uso **não tem data de expiração**. Isso remove a renovação a cada ~60 dias que o mecanismo anterior exigia, mas **não remove** a obrigação de detectar invalidez: token revogado ou sem permissão gera 401/403, e nesse caso a automação marca `conexao_meta = expirada`, avisa na tela e por e-mail, e **preserva o último dado bom** (RN-F3-027).
6. **Reversão:** apagar o segredo `META_ACCESS_TOKEN` e desligar o cron. As coleções permanecem como histórico; nada é apagado.

## O que NÃO muda

- **As seis vedações do aceite de 28/10 continuam valendo integralmente:** nada de ativar, pausar, criar ou editar campanha, conjunto ou anúncio; nada de alterar orçamento ou criativo; nada de ler formulário de lead ou dado pessoal de contato; nada de escrever em qualquer conta; nada de enviar dados a outro projeto que não o "repositório adapta".
- **RN-F3-025, RN-F3-026, RN-F3-027, RN-F3-028 e RN-F3-029** seguem vigentes e não são dispensadas por este documento.
- **B3-LGPD-01** segue intocado: envios do formulário da landing page continuam fora do escopo desta automação.
- **O aceite de 28/10 permanece no histórico** e não é revogado — este documento apenas estende a cobertura ao mecanismo efetivamente implementado.

## Limites

- Vale **exclusivamente para a F3-T12 e a F3-T13**, leitura de métricas de campanha.
- Qualquer outro uso do token, de outra conta, ou de outro projeto exige **novo aceite escrito**.
- Não autoriza escrita na Meta em nenhuma hipótese, mesmo o token tendo escopo `ads_management`.

## Evidência da primeira execução com a nova fonte

Registrada em 30/10/2026 02:47 UTC, pelo retorno do próprio hook:

| Item | Resultado |
|---|---|
| Campanhas lidas | 10 |
| Linhas gravadas | 70 (10 campanhas × 7 dias) |
| Criadas / atualizadas | 0 / 70 |
| Erros de gravação | nenhum |
| Escrita na Meta | nenhuma (`somente_leitura_meta: true`) |
| Janela | 22/10 a 28/10/2026 |

## Pendência declarada

O **CA-3-16 depende da conferência visual do Champion** no Gerenciador de Anúncios, comparando as 10 campanhas da janela 22–28/10. A leitura automatizada está comprovada; a igualdade dos números contra o Gerenciador **ainda não foi atestada por humano**. Este documento não substitui essa conferência.

---

*Aceite reafirmado registrado pelo Maestro em 30/10/2026, com base na resposta do owner ao formulário. Nenhuma alteração executada na Meta além das leituras citadas. Versão em execução: Skip 51414 v0.0.117, ref 6e27a41.*
