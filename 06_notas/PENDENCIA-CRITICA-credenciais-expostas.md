# PENDÊNCIA CRÍTICA BLOQUEANTE — credenciais expostas no código público

> **Status: ABERTA · BLOQUEIA o uso real do sistema**
> Registrada em 11/11/2026 19:06 por decisão do owner.
> **Não é opcional. Não pode ser esquecida. Nenhum uso real antes de resolver.**

## Regra de bloqueio

**O sistema NÃO pode ser liberado para uso real enquanto esta pendência estiver aberta.**

Liberar para uso real inclui, sem exceção:
- publicar em produção;
- conectar dados reais de clientes, candidatos ou campanhas;
- criar contas para pessoas da empresa que não sejam de teste;
- sair do ambiente de homologação.

**Até a resolução: somente dados fictícios.**

## O que está exposto

A senha do usuário de teste `humano-sintetico-aprovador-01@f2.invalid` — senha `F2-Sintetico-2026!` — está escrita em texto aberto no código do aplicativo, que é **público por definição**: qualquer navegador que abra o sistema baixa o arquivo inteiro.

**Ocorrências verificadas:** 6 cópias da mesma credencial, em 6 telas.

| # | Arquivo | Tela |
|---|---|---|
| 1 | `src/pages/CampanhasF3Page.tsx` | Campanhas (desde 28/10) |
| 2 | `src/pages/PipelineF3T01Page.tsx` | Pipeline (T01) |
| 3 | `src/pages/ProvaF3T13Page.tsx` | Prova da conexão Meta |
| 4 | `src/pages/ExperimentosF2.tsx` | Experimentos (Fase 2) |
| 5 | `src/components/seg/AcessoRequerido.tsx` | Visão geral (Inicial) |
| 6 | `src/components/seg/ProtecaoDeRota.tsx` | Funil |

**Poder da credencial:** papel `champion` — o mais alto disponível. Permite ler tudo, aprovar e bloquear na Fase 2, criar e alterar decisões.

**Limites que as Etapas 1 e 2 já impõem:** não cria conta, não muda papel, não apaga demanda.

**Laudo completo:** `artifacts/LAUDO-credenciais-expostas-2026-11-11.md`

## O que tem de ser feito antes de liberar

Três etapas obrigatórias, nesta ordem:

1. **Remover todas as senhas do código** — as seis cópias. Cada pessoa digita a própria credencial; nenhuma senha fica no frontend.
2. **Revogar as credenciais antigas** — trocar a senha do usuário de teste e desativá-lo, ou apagá-lo. Uma senha que já foi pública não volta a ser segura.
3. **Validar a segurança dos acessos** — conferir de novo o arquivo público procurando segredos; testar que ninguém entra sem credencial válida; confirmar que as proteções das Etapas 1, 2, 5 e 6 continuam ativas.

## Onde isso entra no plano

- **Etapa 4** (contas reais) é o gatilho: ao criar as contas verdadeiras, as credenciais saem do código e cada pessoa usa a sua.
- **Etapa 5** (fechar leitura pública de demandas) só faz sentido **depois** dos itens 1 e 2 acima. Fechar a porta com a chave colada na parede não protege nada.

## Verificação de que a pendência continua aberta

Enquanto esta pendência estiver aberta, a resposta ao comando abaixo deve continuar **vazia**. Se vier alguma ocorrência, nada foi resolvido:

```bash
curl -s "https://repositorio-adapta-cc556--preview.goskip.app/funil" \
  | grep -o "index-[A-Za-z0-9_-]*\.js" | head -1
# depois:
curl -s "https://repositorio-adapta-cc556--preview.goskip.app/assets/<arquivo>.js" \
  | grep -c "F2-Sintetico-2026!"
```

**Esperado ao final:** `0`

---

## Histórico

- **11/11/2026 19:05** — owner pediu verificação de credenciais expostas antes de liberar a próxima rota.
- **11/11/2026 19:05** — verificação feita: 6 cópias encontradas. Nenhum outro segredo exposto (token Meta, chave do Maestro, token de leads, SMTP: nenhum).
- **11/11/2026 19:06** — owner decidiu: seguir com a Etapa 3 e tratar na Etapa 4; registrada como **pendência crítica obrigatória**.
