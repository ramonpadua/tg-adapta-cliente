# AP-2026-11-04-1453 — Prova de falha tem de ser falha de verdade

**Task:** F3-T13 · **Data:** 04/11/2026 · **Origem:** defeitos encontrados ao rodar os testes com o token oficial

## Sinal observado

A F3-T13 pedia provar o comportamento quando a conexão Meta cai. A implementação original tinha um "modo ensaio" que simulava a falha sem chamar a Meta. O QA do Skip passou, a bateria de ensaio passou, e mesmo assim **quatro defeitos reais** estavam escondidos:

1. O ensaio gravava `conexao_meta` e disparava e-mail — o teste sujava o estado que media.
2. A faixa de aviso mostrava a hora crua em UTC (`2026-11-04T17:57:03.485Z` onde eram 14:57).
3. **Token expirado de verdade volta HTTP 400 com `error.code = 190`, não 401.** O código só tratava 401/403 como "expirada": uma expiração real seria classificada como falha genérica.
4. Os textos dos cenários prometiam "e-mail e faixa de aviso" depois que o ensaio deixou de fazer isso.

Nenhum dos quatro apareceu em teste automático. Todos apareceram ao (1) executar o caminho real e (2) ler a tela comparando com a fonte.

## Padrão reutilizável

**Simulação prova o que o código faz no ramo simulado; não prova o que a Meta/serviço devolve.** Quando o critério de aceite fala de reação a uma falha externa, a evidência tem de vir do caminho real — e o caminho real precisa ser projetado para ser seguro: credencial inválida deliberada no lugar da boa, execução, leitura do efeito, restauração.

Regra que fica:

- **Ensaio não escreve nada.** Se o modo de teste grava saúde, log, histórico ou dispara aviso, ele contamina a evidência e engana quem lê a tela depois. Cortar antes do primeiro efeito colateral e declarar `estado_alterado: false` no retorno.
- **Para provar reação a falha externa, use falha externa real**, com um valor deliberadamente inválido no lugar do bom — e verifique o efeito no log do próprio sistema, não só o retorno da chamada.
- **Ao corrigir, limpe o resíduo** que o teste já gravou, com uma migração que apaga só o que se identifica como ensaio pela própria mensagem. Preferir deixar passar uma linha de ensaio a apagar dado real.
- **Alinhe o texto da tela ao comportamento novo.** Prometer o que não acontece é o mesmo defeito pela outra ponta.

## Códigos de erro: conferir na fonte, não presumir

O código assumia 401 para credencial inválida porque é o que a RFC sugere. A Graph API devolve **400 com `code: 190`**. Comprovado com três tokens inválidos diferentes ("Malformed access token", "could not be decrypted", "Invalid OAuth access token"). Antes de ramificar por código HTTP de um serviço externo, testar o serviço com um valor inválido e ler o código que ele realmente devolve.

## Evidência

- `06_notas/changelog-2026-11-04-f3-t13-concluida.md` — os quatro defeitos com versão e causa.
- Log do sistema (04/11 17:57:03 UTC): `email sent`, assunto "Campanhas Meta: Conexao Meta expirada".
- Captura da tela `/campanhas` às 14:57 BRT com a faixa de aviso e os números anteriores intactos.
