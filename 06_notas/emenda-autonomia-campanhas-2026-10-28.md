# Emenda proposta — Autonomia de gestão de campanhas (Meta + LinkedIn)

**Data:** 2026-10-28 · **Origem:** decisão do Champion/owner (Luiz Carlos Manni) em 28/10/2026 · **Natureza:** mudança de escopo com emenda à SPEC-F3-006 (RN-F3-025) e ao bloqueio B3-META-01 · **Status:** PROPOSTA — aguardando emenda do consultor. **Nada implementado.**

## 1. Decisão do Champion (registro literal)

Em 28/10/2026 o Champion declarou: *"a ideia do projeto é que seja tudo automático e só acessarmos o dashboard para gerenciar. O Maestro será o nosso gestor de tráfego com autonomia para subir, parar, gerenciar por completo as campanhas tanto no meta quanto no linkedin"*.

Verba aprovada pelo Champion em 28/10/2026 (a diretoria não retornou sobre verba; decisão tomada pelo próprio Champion): **R$ 2.700/mês no total — Meta R$ 1.000 + LinkedIn R$ 1.700** (opção 3 confirmada às 21:09).

## 2. O que esta emenda substitui

| Regra vigente hoje | Efeito desta emenda |
|---|---|
| RN-F3-025 — "Somente leitura na Meta; pedido de escrita → recusar e parar" | **Superseded** para as ações da faixa autorizada; permanece válida para todas as demais |
| B3-META-01 — "somente métricas de campanha por esta SPEC" | **Superseded** no que diz respeito a ações de gestão autorizadas |
| SPEC-F3-006 §Limites — "Fora de escopo: qualquer escrita na Meta; LinkedIn e Google (Fase 4)" | **Superseded** para Meta (escrita autorizada conforme §4) e LinkedIn (passa a estar no escopo, condicionado à §7) |

**Preservado sem alteração:** RN-F3-026 (destino único), RN-F3-027 (último dado bom preservado), RN-F3-028 (sem dado pessoal), RN-F3-029 (automação é task), B3-LGPD-01.

## 3. Verba e tetos (valores fechados)

Base cambial: **Ptax BC 28/10/2026 = R$ 5,3420/US$**. Piso da plataforma LinkedIn: **US$ 10/dia por campanha** (LinkedIn Budget Guidance — verificado em 3 fontes em 28/10/2026).

| Canal | Mensal | Diário | Observação |
|---|---:|---:|---|
| Meta Ads | R$ 1.000,00 | R$ 33,33 | Verba definida pelo Champion |
| LinkedIn Ads | R$ 1.700,00 | US$ 10,60 (≈ R$ 56,63) | Acima do piso de US$ 10/dia |
| **TOTAL** | **R$ 2.700,00** | **R$ 90,00** | |

**Teto diário total de todas as contas: R$ 90,00.** Soma verificada: R$ 33,33 + R$ 56,63 = R$ 89,96 ≤ R$ 90,00.

**Margem cambial:** o valor suporta dólar até **R$ 5,6667** (US$ 300/mês no LinkedIn) — folga de **+6,08%** sobre a Ptax de hoje. Se o dólar passar disso, o teto do LinkedIn é reduzido automaticamente para não estourar o total; a redução é registrada na tela.

**Regra do estouro:** qualquer ação cuja projeção ultrapasse o teto diário ou mensal é **recusada, registrada e sinalizada na faixa** — nunca "avisa e segue". A verificação é feita no código antes da chamada ao provedor.

## 4. Faixa de autonomia por ação

| Ação | Nível | Condição |
|---|---|---|
| Pausar campanha que queima dinheiro | **LIVRE** | Somente com gasto > 0 e resultado = 0 no período da regra; registra o motivo |
| Reduzir orçamento | **LIVRE** | Reduzir protege caixa; registra valor antes/depois |
| Aumentar orçamento | **LIVRE até +20%** | Acumulado máximo +50% por campanha ao dia; acima de +20% exige aprovação do Champion |
| Reativar campanha pausada | **APROVAÇÃO** | Reativar assume gasto — decisão do Champion |
| Criar campanha / conjunto / anúncio | **APROVAÇÃO** | Máximo 3 por dia e 5 por semana, mesmo com aprovação |
| Alterar público, criativo, texto, imagem | **PROIBIDO** | Só com o Champion especificando a mudança por escrito |
| Faturamento, cartão, forma de pagamento | **PROIBIDO** | Nunca, em nenhum cenário |
| Alterar o próprio teto ou esta faixa | **PROIBIDO** | A automação não pode afrouxar a regra que a limita |

**Teto por campanha sem aprovação:** R$ 25/dia (Meta) — impede que uma campanha consuma o orçamento das outras.

## 5. Trilha de auditoria (obrigatória)

Cada ação executada grava **seis campos**: data/hora, ação, campanha/conjunto/anúncio afetado, **a regra que disparou**, valor antes, valor depois, e o `execucao_id`. Sem trilha, a ação não é executada — o registro é pré-condição, não log opcional. A trilha é legível pelo Champion na tela e não pode ser apagada pela automação.

## 6. Botão de pânico

Interruptor único no dashboard que **desliga toda autonomia em um clique**, com efeito imediato e sem depender de deploy, do consultor ou de abertura de ticket. Desligado, o Maestro volta a **somente ler e sugerir** — não para de funcionar, para de agir. O estado do interruptor é registrado com data/hora e autor.

## 7. LinkedIn — condição de entrada

O LinkedIn **não entra em operação nesta emenda**. Situação real: app "Talent Group - ETHOS Ads" em **Development Tier** (500 chamadas/24h), escopos atuais `r_ads`/`rw_ads`/`r_ads_reporting`, operação de Campaign Manager manual pelo Champion (JP). Criar campanha por API exige **acesso Standard/Aprovado pelo LinkedIn** — processo externo, prazo fora do nosso controle.

**Regra:** o LinkedIn entra em modo leitura quando o acesso de leitura estiver validado; entra em autonomia **somente após** (a) aprovação do LinkedIn, (b) a mesma trilha de auditoria e tetos, (c) modo sombra cumprido. A verba de R$ 1.700 fica reservada e **não é gasta por nenhum outro canal**.

## 8. Modo sombra (transição — 14 dias)

Antes de ganhar a mão, o Maestro opera 14 dias em sombra: **decide e registra, sem executar**. O Champion compara o que ele faria com o que ele mesmo faria.

**Critérios objetivos de saída da sombra:**
1. Mínimo **10 decisões registradas** no período.
2. **≥ 80% de concordância** com a decisão que o Champion teria tomado.
3. **Zero tentativa** de ultrapassar teto (a verificação roda em sombra também).
4. **Zero ação** que a faixa classifica como PROIBIDO.

Não atingido: mais 14 dias. Atingido: ganha a mão **apenas nas ações LIVRES**; as de APROVAÇÃO continuam exigindo o Champion.

## 9. O que este documento NÃO autoriza

- **Não** autoriza implementação: depende de emenda do consultor na SPEC.
- **Não** altera a F3-T12 em curso, que segue **somente leitura** até a emenda.
- **Não** dispensa B3-LGPD-01 (criar público a partir de lista exige base legal).
- **Não** promete prazo para o LinkedIn.

## 10. Impacto em prazo (estimativa, não compromisso)

| Etapa | Estimativa |
|---|---|
| Emenda do consultor na SPEC | 1 ciclo |
| Implementação Meta (ações + tetos + trilha + pânico) | 1 ciclo |
| Modo sombra | 14 dias corridos |
| LinkedIn (leitura → escrita) | depende de aprovação externa; sem data |
| Google | fora do escopo desta emenda |

## 11. Riscos registrados

- **Financeiro:** o pior caso de um bug de repetição está limitado a R$ 90/dia (R$ 2.700/mês). O teto existe para isso.
- **Reputação:** pausar campanha precisa de explicação pronta ao cliente — a trilha fornece.
- **Política de plataforma:** Meta e LinkedIn vigiam mais automação de escrita que de leitura; conta pode ser restringida. O padrão de ação deve ser conservador.
- **Expectativa de resultado:** R$ 2.700/mês é orçamento de **teste**, não de volume. Contra a meta de 60 vagas R&S/mês, o CPL de decisor (4–30x o de lead comum) não permite escala. Registrado para não haver leitura equivocada do resultado do mês.

## 12. Natureza provisória da verba (registro do Champion, 28/10/2026 às 21:11)

O Champion registrou que **os R$ 2.700/mês são provisórios**: a diretoria ainda não retornou sobre verba e o valor será **atualizado quando ela responder**.

**Consequência para a implementação (requisito, não sugestão):** os valores da §3 não podem ser constantes no código. Eles são **parâmetros** — uma tabela de configuração com histórico de alteração (valor, data, quem alterou, motivo). Trocar a verba é editar o parâmetro, nunca mexer na regra.

O que NÃO muda quando a verba mudar:

- a faixa de autonomia da §4 (livre / aprovação / proibido) — é política, não valor;
- a trilha de auditoria da §5 e o botão de pânico da §6;
- o modo sombra da §8 e seus critérios de saída;
- a regra do estouro da §3 (recusa + registro + faixa), que vale para qualquer valor.

O que muda: apenas o teto mensal, o teto diário derivado e o teto por campanha (que é proporcional).

**Cenários já preparados para a resposta da diretoria:**

| Verba total aprovada | Meta | LinkedIn | O que passa a ser possível |
|---:|---:|---:|---|
| R$ 1.000 | R$ 1.000 | R$ 0 | LinkedIn desligado; canal único |
| R$ 2.700 (atual) | R$ 1.000 | R$ 1.700 | Os dois no piso; dado fraco mas real |
| R$ 3.700 | R$ 2.000 | R$ 1.700 | Meta com tração + LinkedIn no piso |
| R$ 5.000+ | R$ 2.000 | R$ 3.000 (≈US$ 50/dia) | LinkedIn em nível que gera aprendizado |

Quando o Champion trouxer o número da diretoria, a atualização é: (1) novo valor no parâmetro, (2) recálculo automático dos tetos diário e por campanha, (3) registro no changelog com data e origem. Sem nova emenda, porque a política já está aprovada — muda só a régua de gasto.
