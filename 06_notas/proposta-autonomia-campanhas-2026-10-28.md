# Proposta — Maestro como gestor de tráfego com autonomia (Meta + LinkedIn)

**Data:** 2026-10-28 · **Origem:** decisão de produto do Champion/owner em 28/10/2026 ("o Maestro será o nosso gestor de tráfego com autonomia para subir, parar, gerenciar por completo as campanhas tanto no Meta quanto no LinkedIn") · **Status:** PROPOSTA — nada implementado, nada autorizado

## 1. O que muda

O que está aprovado hoje (SPEC-F3-006, RN-F3-025, B3-META-01) é **leitura**: o Maestro lê métricas e mostra na tela; escrever na Meta é recusado e a execução para. O que o Champion quer é **autonomia de gestão** — criar campanha, subir, pausar, ajustar orçamento, no Meta **e** no LinkedIn.

São dois patamares muito diferentes de risco:

| | Leitura (hoje) | Autonomia (proposto) |
|---|---|---|
| Erro máximo | número errado na tela | **dinheiro gasto errado**, campanha parada em dia de pico, anúncio reprovado no ar |
| Precisa de | um passe de leitura | permissão de escrita + regra de limite + rastro de quem fez o quê |
| Quem responde pelo erro | o Maestro, tecnicamente | **a empresa**, perante o cliente e o fisco |
| Reversível? | sim, nada muda | parcialmente: gasto não volta, alcance perdido não volta |

## 2. O que é tecnicamente possível

**Meta:** sim, com a API de Marketing completa — criar campanha/conjunto/anúncio, ativar, pausar, alterar orçamento. As ferramentas existem no conector (METAADS_CREATE_CAMPAIGN, METAADS_CREATE_AD_SET, METAADS_CREATE_AD, METAADS_UPDATE_CAMPAIGN etc.). **Não existe** no conector uma ação de "pausar" isolada listada — é via update de status; verificar antes de prometer.

**LinkedIn:** sim, via Marketing API — mas em **outro patamar de exigência**: o app precisa de acesso Standard/Approved, não Development Tier (hoje o app está em Development, 500 chamadas/24h, e o Campaign Manager é operado manualmente pelo JP). Criar campanha por API exige permissão de escrita aprovada pelo LinkedIn.

## 3. As cinco travas que eu não dispensaria

Sem estas, autonomia total é irresponsável — não por conservadorismo, por aritmética:

1. **Teto de gasto diário e mensal por conta.** Um bug de loop pode subir 50 campanhas em uma noite. O teto é verificado **antes** da ação, no código, e não depende de "a IA se comportar".
2. **Faixa de autonomia por ação.** Escala de 3 níveis, decidida por você:
   - **Livre:** pausar campanha com gasto sem resultado (protege dinheiro).
   - **Com aprovação:** alterar orçamento acima de X%, ativar campanha nova.
   - **Proibido sem você:** criar campanha nova, mudar público, mudar criativo, mexer em faturamento.
3. **Trilha de auditoria.** Cada ação grava: quando, o quê, por quê (a regra que disparou), valor antes/depois, e qual execução. Sem isso ninguém consegue explicar ao cliente por que a campanha parou.
4. **Botão de pânico.** Um lugar único que desliga toda autonomia em um clique, sem depender de mim nem de deploy.
5. **Modo sombra primeiro.** O Maestro **decide e registra** por 2 semanas sem executar; você compara o que ele faria com o que você faria. Só depois ganha a mão. Isso é o que separa autonomia de aposta.

## 4. O que isso custa em prazo

Não é uma task a mais na F3 — é uma **fase nova**, porque muda o contrato de segurança do sistema:

- SPEC nova de autonomia (ações permitidas, limites, aprovação, auditoria, pânico) — 1 ciclo.
- Implementação Meta: criar/pausar/orçamento com teto e auditoria — 1 ciclo.
- LinkedIn: **depende de aprovação do LinkedIn** (semanas, fora do nosso controle) + app OAuth + escrita.
- Modo sombra + calibragem + homologação — 1 ciclo.

Estimativa honesta: **Meta com autonomia em ~2 ciclos; LinkedIn é imprevisível** porque depende de aprovação externa. Não prometo data do LinkedIn antes de saber o status do app.

## 5. Os riscos que precisam estar na mesa

- **Risco financeiro:** autonomia sem teto = exposição direta de caixa. Com teto, o pior caso é conhecido.
- **Risco de reputação:** pausar campanha de um cliente no meio de uma campanha contratada precisa de explicação pronta.
- **Risco de mercado:** as políticas do Meta e do LinkedIn mudam. Automação de escrita é mais vigiada que leitura — conta pode ser restringida por comportamento automatizado.
- **Risco de LGPD:** criar público a partir de lista exige base legal. Não entra sem política (B3-LGPD-01).

## 6. Decisão que eu preciso do Champion (por escrito)

1. **Confirma a mudança de escopo?** Isso supersede a RN-F3-025/B3-META-01 (somente leitura) e precisa ser registrado como emenda — não posso alterar uma regra aprovada sem emenda.
2. **Qual teto?** Valor diário e mensal, por conta, que o Maestro nunca ultrapassa. Sem número, não implemento.
3. **Qual faixa de autonomia** para cada ação (livre / com aprovação / proibido)?
4. **Modo sombra de 2 semanas: aceita?** (recomendo fortemente)
5. **LinkedIn:** autoriza levantar primeiro o status real do app e a exigência de aprovação, antes de prometer qualquer coisa?

## 7. Recomendação

Faço nesta ordem e não mudo: **(a)** fechar a F3-T12 como está — leitura funcionando e provada, porque ela é a base de qualquer autonomia (sem número confiável não existe decisão automática); **(b)** SPEC de autonomia com as cinco travas; **(c)** modo sombra; **(d)** só então a mão na Meta; **(e)** LinkedIn em paralelo apenas no levantamento, sem promessa de data.

Autonomia sem número confiável é chute automatizado. E chute automatizado gasta dinheiro mais rápido que gente.
