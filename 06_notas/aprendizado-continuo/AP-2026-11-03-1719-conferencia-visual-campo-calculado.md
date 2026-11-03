# AP-2026-11-03-1719 — Conferência visual de campo calculado (três defeitos seguidos)

- **Sinal**: teste automático que confere **contagem de linhas gravadas** e lista de erros vazia **não prova que o conteúdo está correto**. Os três defeitos da tela Campanhas passaram por todos os meus testes verdes e só apareceram quando o owner abriu a tela e leu os números.

- **Evidência (F3-T12, 30/10 a 03/11)**:
  1. `normalizarStatus` fazia `String(v === undefined || v === null || v === '')` — convertia o **resultado booleano** da comparação. `String(false)='false'`, nenhum status batia, **80/80 linhas gravadas como `desconhecido`**. Os testes diziam "70 linhas gravadas, `erros: []`".
  2. A tela não tinha porta de entrada própria: sem sessão, as coleções (corretamente protegidas por `listRule: @request.auth.id != ''`) devolviam lista vazia e a tela mostrava "Sem informação da conexão" — sintoma sem causa nem ação, apontando para a automação quando o problema era a sessão do usuário.
  3. `resultados` somava **todos os 13 itens de `actions`** da Meta (engajamento + conversão + duplicata `omni_*`): mostrava **654** onde o real era **113**, violando a própria regra impressa no rodapé da tela (RN-F3-008: volume e resultado separados).

- **Agravação**: em duas das três vezes eu declarei "corrigido" com base em teste que não provava o que eu afirmava. Na terceira, o owner mandou um print de 49 segundos antes da correção entrar no ar e eu quase tratei como se o defeito tivesse voltado — a prova disso foi comparar `snapshot_em` do banco com o carimbo do print.

- **Como aplicar (regra)**:
  1. Campo **calculado** (soma, contagem, normalização, tradução de código) nunca se confere contra si mesmo: conferir contra a **fonte externa**, valor por valor, e nos casos-limite (lista vazia, valor ausente, tipo inesperado).
  2. Antes de dizer "corrigido": abrir a **tela renderizada** e procurar o valor errado no texto da página — não no banco, não no log, não no retorno da API.
  3. Quando o owner manda print, comparar **carimbos de tempo** (`snapshot_em` × rodapé do print) antes de afirmar que o defeito persiste.
  4. Toda tela que lê coleção autenticada precisa da **própria porta de entrada**; não herdar sessão de outra tela nem exibir "sem dados" quando o problema é autorização.

- **Reutilizável em**: qualquer tela de painel/métrica (F3-T03/T04 Gargalos, F3-T10/T11 Funil, dashboards do Hub) e qualquer hook que grave campo derivado de resposta de API externa.
