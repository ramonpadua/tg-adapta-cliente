# Diagnósticos do Adapta — índice (11/11/2026)

Este arquivo existe porque a publicação do `.adapta-cliente/estado-atual.md` truncou ao passar de ~12 KB e perdeu as seções de diagnóstico. Os diagnósticos originais de 11/11 já estavam publicados em arquivos próprios e não foram perdidos:

| Diagnóstico | Arquivo | Status |
|---|---|---|
| Funil travado em "Carregando" (16:10) | `06_notas/diagnostico-funil-carregando-2026-11-11.md` | **resolvido** — owner confirmou às 16:21 que o Funil abre no preview |
| Páginas em branco (17:20) | `06_notas/diagnostico-paginas-em-branco-2026-11-11.md` | aguardando confirmação do owner sobre o endereço usado |
| Análise de segurança (12:40) | `06_notas/analise-seguranca-adapta-2026-11-11.md` | base das 6 etapas em curso |

## Resumo do diagnóstico das páginas em branco (17:20)

**Causa comprovada:** o endereço de **produção** foi acessado. O app nunca foi publicado (`isPublished: false`), e a página devolve "Produção não publicada — Esse app ainda não foi publicado". O elemento `root` nessa URL tem **0 bytes**.

| URL | Estado |
|---|---|
| `repositorio-adapta-cc556--preview.goskip.app` | **funciona** — todas as homologações foram feitas aqui |
| `repositorio-adapta-cc556.goskip.app` | **nunca publicado** — só o aviso do Skip |

**Evidências no preview (sessão limpa):** `/funil` root 101.964 B com 13 cartões; `/` root 61.478 B com painel; `/pipeline-t01` root 20.435 B. As 4 rotas devolvem HTTP 200 com HTML de 1.268 B apontando para `assets/index-DQVPeI_P.js`, que responde 200 com 2.028.699 bytes.

**A v0.0.160 não introduziu o problema:** `lastDevBuildRef: af1622a` é o build dela, e ele renderiza. A etapa 2 mexeu só em regra de acesso, nenhum arquivo de tela.

**Dados e regras intactos:** health 200 em 35 ms; login funciona; demandas 13, propostas 2, vagas 2, pipeline_log 9, campanhas_meta 200, experimentos_f2 3, decisoes_f2 6; 1 conta `champion`. Proteções ativas: escrita anônima em demandas 404, criação anônima de conta 403, `role` no pedido 404, leitura anônima de demandas 13 registros (aberta até a etapa 5).

**Correção necessária:** nenhuma no preview. A ação é usar o endereço com `--preview`. Publicar em produção fica para depois da etapa 5.

**Lição de processo:** o `.adapta-cliente/estado-atual.md` deve ficar abaixo de ~12 KB, ou o conteúdo precisa ir em arquivos separados. A partir daqui, seções longas vão para `06_notas/` e o estado só aponta para elas.
