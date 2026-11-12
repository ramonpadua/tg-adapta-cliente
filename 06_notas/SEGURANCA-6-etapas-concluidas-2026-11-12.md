# Segurança do ETHOS — as 6 etapas concluídas (12/11/2026)

**Autorizado em** 11/11/2026 12:51, em seis etapas com homologação entre elas.
**Concluído em** 12/11/2026 · **Versão final:** v0.0.186

---

## O problema que deu origem

Em 11/11, um teste do próprio Maestro alterou uma demanda **sem estar logado**. A investigação encontrou um sistema com as portas abertas: a coleção principal aceitava criação, alteração e exclusão anônimas; o cadastro de contas era público; qualquer pessoa podia se promover a administrador; e uma senha de teste estava publicada no código que todo navegador baixa.

Laudo inicial: `06_notas/analise-seguranca-adapta-2026-11-11.md`.

---

## As seis etapas

| # | O que foi fechado | Versão | Homologação |
|---|---|---|---|
| 1 | Escrita anônima em `demandas` (criar, alterar, apagar) | v0.0.159 | ✅ 11/11 16:21 |
| 2 | Cadastro público de contas e autoatribuição de papel | v0.0.160 | ✅ 11/11 17:24 |
| 3 | Login em todas as 7 rotas com dado | v0.0.170 | ✅ 11/11 19:04 |
| 4 | Contas reais, credenciais fora do código, conta de teste desativada | v0.0.181 | ✅ 12/11 10:34 |
| 5 | Leitura pública de `demandas` | v0.0.184 | ✅ 12/11 11:12 |
| 6 | Exclusão de decisões da Fase 2 — bloqueada para **todos** | v0.0.186 | aguardando |

---

## Estado final das regras

### `demandas` — o registro principal

| Regra | Estado |
|---|---|
| ler lista / ler registro | exige login |
| criar / alterar | exige login |
| apagar | **ninguém apaga pela API** |

### `users` — contas

| Regra | Estado |
|---|---|
| criar conta | **ninguém cria pela API** (só pelo painel do Skip) |
| alterar | só a própria conta, e **sem tocar no campo `role`** |
| ler | cada um vê só a si mesmo |

### `decisoes_f2` — decisões da Fase 2

| Regra | Estado |
|---|---|
| ler | exige login |
| criar | exige login, e nasce sempre `pendente` |
| alterar (registrar/revogar) | exige `champion` ou `delegado_f2` |
| **apagar** | **ninguém — nem champion** (decisão do owner: preservar rastreabilidade) |

### As outras dez coleções

Todas exigem login para ler; escrita bloqueada ou restrita por papel. Mapa completo em `06_notas/analise-seguranca-adapta-2026-11-11.md`.

---

## Contas reais

| Conta | Papel | Estado |
|---|---|---|
| `jpaulo@talentgroup.com.br` | champion | ✅ define a própria senha e acessa |
| `manni@talentgroup.com.br` | champion | ✅ define a própria senha e acessa |

**Conta de teste `humano-sintetico-aprovador-01`:** desativada em três camadas (não verificada, senha aleatória, e-mail inválido). Mantida no banco para preservar o rastro de autoria — não apagada.

---

## Pendência crítica das credenciais — ENCERRADA

6 cópias da senha foram removidas do código. Verificação objetiva no arquivo público: **zero** ocorrências da senha, do e-mail de teste e de qualquer token.

Documento: `06_notas/PENDENCIA-CRITICA-credenciais-expostas.md`.

---

## O que continua valendo

- **Somente dados fictícios** até a política LGPD ser aprovada pela direção.
- **Produção nunca publicada** — o sistema roda no preview.
- **Nada foi apagado** do banco em nenhuma das etapas: 13 demandas, 2 propostas, 2 vagas, 9 eventos de histórico, 6 decisões, 4 aprovações, 210 linhas de campanha.

---

## Reversão

Cada migration tem função de reversão própria. Os estados originais estão em `work/seguranca/`:

| Pasta | Conteúdo |
|---|---|
| `backup-regras-ANTES.json` | regras de demandas, users e decisoes_f2 antes de tudo |
| `backup-regras-ETAPA2.json` | regras de users antes da etapa 2 |
| `backup-v0167/`, `backup-v0169/`, `backup-etapa4b/`, `backup-etapa5/`, `backup-etapa6/` | código e bundles de cada fase |

---

## Nota sobre o processo

Três incidentes aconteceram no caminho, todos registrados:

1. **Funil em branco (v0.0.161–166)** — o Maestro tentou aplicar login *dentro* de um componente de 900 linhas homologado e, ao desfazer, apagou partes dele. **Lição adotada:** proteger por fora da página, nunca mexer no corpo de componente aprovado.
2. **Dois alarmes falsos sobre contas não criadas** — a regra `listRule` de `users` (cada um vê só a si) fazia a consulta devolver apenas a conta logada. **Lição:** consulta vazia não prova ausência.
3. **Publicação truncada do estado do projeto** — acima de ~12 KB, o arquivo perde o final silenciosamente. **Lição:** seção longa vai em arquivo separado; o estado só aponta.

---

## Próximo passo

Voltar à **Fase 3**, com três tarefas liberadas: F3-T03 (gargalos), F3-T05 (passagens entre áreas) e F3-T11 (provas do Funil). Aguardando autorização do owner.
