# Changelog — F3-T10 concluída (11/11/2026)

> Arquivo separado porque `changelog.md` tem 69 KB e a publicação por API trunca acima de ~30 KB.

- 2026-11-11 · [João Paulo] · **Task F3-T10 concluída**: tela **Funil** entregue em cinco etapas, cada uma homologada pelo owner antes da seguinte. **E1** colunas e contagens (aprovada 04/11, "E1 aprovada"); **E2** cartão completo (04/11, "tudo confere"); **E3** filtros (05/11, "ok bateu"); **E4** Jornada (05/11, "ok bateu"); **E5** arrastar com evidência (11/11, "E5 aprovada").

## A prova final foi do owner

Ele arrastou o cartão **"Serviços Delta Ltda."** (`PIPE-F3-IN-001`) da coluna **Oportunidade** para **Proposta**, escrevendo a evidência no campo obrigatório. Evidência observada na fonte: o registro passou a `estado = proposta`, a evidência foi gravada, e o histórico append-only registrou o evento `oportunidade → proposta` às 15:11:30 de 11/11. As colunas acompanharam: Oportunidade caiu de 3 para 2, Proposta subiu de 0 para 1, soma total 13 = 13.

É a demonstração do **CA-3-14** na sua forma mais direta: "com evidência, arrastar gera transição registrada".

## Quatro defeitos reais corrigidos

1. **Zero falso sem sessão (E2, v0.0.150).** A tela mostrava "0 propostas · 0 vagas" e "área: não informada" em todos os cartões. Causa: eu supus que, sem sessão, a leitura de `propostas`/`vagas` daria erro e cairia no `catch`. Não dá — o PocketBase filtra e devolve **HTTP 200 com lista vazia**. O `catch` nunca disparava e a tela apresentava vazio como se fosse zero real, quando `PIPE-F3-IN-001` tem 1 proposta e 1 vaga. Corrigido conferindo `pb.authStore.isValid` **antes** de ler os filhos: sem sessão a tela avisa, em vez de inventar zero.
2. **Contador fora do filtro (E3, v0.0.152).** Com o filtro "tipo = desconhecido" a tela mostrava **"Demandas 2" e "Prazo vencido 9"** ao mesmo tempo. O contador "Prazo vencido" era o único dos cinco que lia o dado bruto em vez do filtrado. Corrigido: os cinco acompanham o filtro. **Achado no print do owner**, não pelos testes automáticos — que passaram nas duas versões.
3. **Hora crua em UTC na faixa (herdado da T12, corrigido em 04/11).** A faixa mostrava `2026-11-04T17:57:03.485Z` onde em Brasília eram 14:57.
4. **Tela em branco (E5, v0.0.157 → v0.0.158).** A primeira versão da E5 deixou a página vazia: `confirmarMover` usava `carregar` na lista de dependências **antes** de `carregar` ser declarado — erro de execução que o build do Skip **não pega** (sintaxe e tipos passam). Corrigida a ordem dos hooks.

## O que a task entregou, em uma linha

Uma tela onde cada demanda aparece na sua etapa, com origem, canal, campanha, serviço, área, dono, próxima ação e prazo; filtros que recalculam os números junto com os cartões; um painel de Jornada com a história completa; e o arrastar que passa pela transição real do pipeline, com evidência obrigatória — sem inventar mecanismo próprio.

## Limites declarados na própria tela

- **`campanha` é texto livre** na demanda, sem vínculo com a coleção `campanhas_meta` da F3-T12. A Jornada mostra o nome como está gravado e diz isso ao lado, em âmbar. Criar o vínculo está fora da SPEC desta task.
- **O filtro "incluir dados de teste" começa marcado.** A RN-F3-023 trata de *indicadores oficiais*; o Funil é quadro operacional e hoje 13 de 13 registros são sintéticos — começar excluindo abriria a tela vazia. O selo TESTE segue em cada cartão.

## Ocorrências preservadas por decisão do owner

Ver `06_notas/registro-eventos-teste-e5-2026-11-05.md`. Eventos de teste técnico ficaram no histórico e **não representam movimentação comercial**; a decisão de preservá-los foi do owner, e o registro existe para que quem auditar encontre a explicação antes de desconfiar do sistema.

## Nada foi criado no banco

Nenhuma coleção, campo, índice ou migration de estrutura nesta task. O Funil é uma visão nova sobre dados que já existiam (RN-F3-020). As migrations 0036/0037/0038 foram tentativas de limpeza de resíduo de teste e não tocaram schema.
