# Changelog — F3-T13 concluída (04/11/2026)

> Arquivo separado porque `changelog.md` tem 69 KB e a publicação por API trunca acima de ~30 KB.

- 2026-11-04 · [João Paulo] · **Task F3-T13 concluída**: provas do CA-3-17 executadas com falha REAL (credencial inválida deliberada no caminho real, não simulação). Evidências observadas: a Meta recusou com erro de credencial; a execução gravou saúde `expirada`; o e-mail foi enviado e está no log do sistema (`email sent`, assunto "Campanhas Meta: Conexao Meta expirada", 04/11 17:57:03 UTC = 14:57 BRT); a tela `/campanhas` mostrou a faixa "Conexão com a Meta expirada"; as 130 linhas do retrato anterior permaneceram intactas; a reexecução com a credencial boa deu 0 criadas e 70 atualizadas; a auditoria confirmou 0 escrita na Meta e destino único. Homologação do owner: "Aprovado — pode concluir a T13".

## Defeitos reais encontrados e corrigidos neste ciclo

1. **O ensaio sujava o estado real (v0.0.140).** O botão "Rodar os cinco" chamava `registrarSaude()` e `avisarChampion()`: gravava 6 linhas de 'erro' em `conexao_meta` e disparava e-mail de conexão expirada com a conexão saudável. A tela passou a dizer "Saúde da conexão: erro". Corrigido: ensaio não grava saúde nem envia e-mail. Resíduo removido pela migration 0035 (apaga só linhas cuja mensagem começa com "Ensaio").
2. **Hora crua em UTC na faixa de aviso (v0.0.146).** A faixa mostrava "Expirou em 2026-11-04T17:57:03.485Z" onde em Brasília eram 14:57. Criada `formatarInstanteBR()` no núcleo, com 4 casos de teste (incluindo virada de dia) — todos passaram.
3. **Token expirado volta HTTP 400/código 190, não 401 (v0.0.145).** Comprovado contra a Graph API com três tokens inválidos diferentes. O código só tratava 401/403 como 'expirada'; uma expiração real cairia em "erro genérico" e a faixa mandaria procurar defeito onde faltava reconectar. Corrigido: HTTP 400/401 com `error.code = 190` é tratado como credencial expirada.
4. **Textos dos cenários prometiam o que não acontecia (v0.0.143).** Os cenários diziam "faixa de aviso e e-mail" e "erro registrado" depois que o ensaio deixou de fazer isso. Alinhados ao comportamento real.

## Prova de credencial

O token do segredo `META_ACCESS_TOKEN` e o arquivo `credentials/meta_token.json` são o MESMO (impressão digital sha256 `7080ebf8c792`), principal João Paulo Oliveira, sem expiração. O token **tem poder de escrita** (declarado pelo owner: já criou e excluiu campanhas); a **automação** só faz GET — duas chamadas, `/campaigns` e `/insights`. As duas coisas passaram a aparecer separadas na tela de prova.
