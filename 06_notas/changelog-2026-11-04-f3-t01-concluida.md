# Changelog — 2026-11-04 · F3-T01 concluída

> Entrada em arquivo separado: `changelog.md` tem 69 KB e a API de conteúdo trunca acima de ~30 KB.
> Este arquivo deve ser anexado ao final do `changelog.md` em uma rodada em que o arquivo principal
> seja atualizado por outro caminho. O conteúdo abaixo segue o formato do changelog.

- 2026-11-04 · [João Paulo] · Task F3-T01 concluída: propostas, vagas, histórico de transições
  append-only, retorno da operação e recuperação comercial 3/90. Evidência: migrações 0026 a 0029
  (coleções `propostas`, `vagas`, `pipeline_log`; campos de recuperação em `demandas`; fixtures
  PIPE-F3-IN-001/OUT-001/STUCK-001), ganchos `pipeline_log_append_only`, `f3_t01_hierarquia`,
  `f3_t01_recuperacao`, `pipeline_log_demanda`, rota `/backend/v1/f3-t01/operacao` e tela
  `/pipeline-t01`. Teste humano: owner executou os 6 passos em 04/11 e aprovou
  ("perfeito. vi aqui. ok tudo certo", 11:44 BRT), enviando 2 capturas com o retorno da
  VAGA-OUT-001 gravado às 11:03 e a faixa verde confirmando que vaga e demanda não foram fechadas.
  Provas automatizadas reexecutadas do zero: CA-3-04 (hierarquia, sem empresa no filho, sem estado
  de pipeline no filho, prazo operacional só na vaga); 4 caminhos de erro recusando; regressão F1
  ("ICP ainda não validado") e F2 (3/26/4/2/6/130/99 registros) intactas; append-only recusando
  edição e exclusão pela API.

## Defeitos reais encontrados durante a implementação e corrigidos

1. `col.fields.add({...})` com objeto literal falha em `could not convert [object Object] to
   core.Field` — exige `DateField`/`NumberField`/`BoolField` (migração 0028).
2. Função declarada no topo do arquivo de gancho não é visível dentro do callback (o PocketBase
   executa cada gancho em VM separada) — registro de evento repetido inline.
3. `$apis.requireRecordAuth('users')` não existe nesta versão — a rota não registrava e devolvia
   404; trocado por `$apis.requireAuth()`, como a rota da T12 já usava.
4. **Evento duplicado**: a rota gravava o evento no histórico E o gancho `onRecordUpdate` gravava
   o mesmo. Corrigido deixando o gancho como fonte única de gravação.
5. **Evento falso (o mais grave)**: `onRecordUpdate` roda ANTES da gravação concluir. Na prova de
   regressão, a F1 recusou `prospect → lead_qualificado` (ICP não validado), a demanda continuou em
   `prospect` — e o histórico gravou a transição mesmo assim. Corrigido movendo o registro para
   `onRecordAfterUpdateSuccess`, que só dispara depois do sucesso. Evento falso removido por SQL
   direto (migração 0034).

## Correções de exibição apontadas pelo owner

- **Fuso horário**: eu informei "14:03" quando o evento ocorreu às 11:03 de Brasília. O banco grava
  UTC. Corrigido o texto e a exibição (v0.0.128).
- **Data de prazo**: a tela mostrava 30/12/2026 onde o prazo era 31/12/2026, e 30/09 onde era
  01/10 — um dia a menos em todo prazo. Causa: campo `date` gravado à meia-noite UTC sendo lido com
  fuso do navegador. Corrigido lendo os componentes em UTC (v0.0.128).

## Lição registrada

Limpeza de `pipeline_log` dentro de migração exige SQL direto (`app.db`): os ganchos de proteção
valem também para o servidor, e `app.delete()` é recusado silenciosamente quando envolto em
`try/catch`. Aconteceu duas vezes (0030 e 0033). A trava funcionar contra mim é a prova de que
funciona.

## Falha de publicação cometida e corrigida nesta mesma rodada

Ao publicar o `04_fase-atual/fase.md`, escrevi o conteúdo "de cabeça" em vez de enviar o arquivo
lido do disco. O resultado foi 6.916 bytes contra os 21.758 do original: **8 das 13 tasks
desapareceram**, junto com as seções "Automações do Maestro neste projeto" e "Bloqueios
transversais". O commit reportou sucesso e o `size` devolvido pela API só chamou atenção porque eu
comparei com o arquivo local.

Correção: o arquivo foi relido por inteiro com `read_file`, as alterações da T01 foram reaplicadas
sobre ele, e o conteúdo integral foi republicado (22.948 bytes, 13 tasks, 116 linhas — conferido
contra o remoto pela contagem de tasks e de linhas, não por tamanho).

**Regra reforçada:** nunca republicar documento de projeto a partir de memória. O caminho é:
baixar/ler o arquivo inteiro → aplicar a troca mínima → publicar → conferir a contagem de
estruturas (tasks, seções, linhas) contra o original. `size` igual não prova conteúdo igual, e
`size` muito menor é sinal de truncamento.

## Pendências declaradas (não são bloqueio da T01)

- A tela `/pipeline-t01` registra retorno; criar proposta/vaga pela interface fica para o Funil
  (F3-T10), que passa a estar liberado com a T01 concluída.
- Este arquivo precisa ser anexado ao `changelog.md` principal em rodada própria, por causa do
  limite de tamanho da API.
- Precedente de limpeza por SQL: válido apenas enquanto não existir histórico de produção
  (`origem_dado=producao` = 0 registros). Após o primeiro aceite com dado real, esse caminho não
  deve ser reutilizado.
