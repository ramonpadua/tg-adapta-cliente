# Orientação ao cliente — consolidação documental de 29/10/2026

A E-F3-001-04 está refletida na SPEC, fase, Tasks Gerais, matriz e fixtures. Não há nova decisão D1–D6 a aguardar. Os registros antigos permanecem históricos e não prevalecem sobre esta consolidação.

## O que o cliente deve fazer

1. Abrir o Maestro usado no projeto e pedir a releitura da SPEC F3-001, fase.md, Tasks Gerais, matriz, fixtures e estado atual atualizados. Conferir que ele identifica o retorno na vaga + histórico, substituição encerrada com os dois motivos e a regra de 3 tentativas/90 dias/confirmação do Comercial. Não é necessário editar arquivos manualmente.
2. Manter uma task ativa por vez. A task registrada é F3-T12 (Campanhas): concluir leitura real e comparação com Gerenciador; se depender da reconexão Meta e a prioridade for T01, registrar pausa segura explícita da T12, mantendo o cron pausado e as evidências.
3. Pedir análise do que falta na F3-T01 com base no aplicativo atual. Preservar E2 origem_dado já aceita; não repetir migration nem dar tudo como concluído. Aguardar a explicação do próximo trecho e autorizar sua execução no Maestro.
4. Conferir no Skip o resultado do trecho, executar o roteiro de teste e confirmar o que passou ou falhou. Se falhar, corrigir e retestar o mesmo trecho; só concluir F3-T01 com todas as provas/aceites exigidos. Sem pedir aprovação do consultor a cada passo técnico já coberto pela SPEC.
5. Depois do aceite integral da F3-T01, seguir F3-T10 (Funil), conforme dependências vigentes.

## Texto para colar no Maestro

> Leia a consolidação documental de 29/10/2026 da E-F3-001-04: SPEC F3-001, fase.md, 00-Tasks_Gerais.md, matriz, fixtures e estado atual. D1–D6 já foram decididas; não repita essas perguntas. Confirme qual task está ativa. Se for F3-T12, mostre o que falta para concluí-la ou me ofereça registrar sua pausa segura antes de retomar a F3-T01, sem executar duas tasks ao mesmo tempo. Depois, confira no aplicativo o que da F3-T01 já existe e preserve os aceites anteriores, incluindo E2 origem_dado. Explique o próximo trecho pendente de E2-coleções/E5/E6 e peça minha autorização antes de implementar. Ao terminar, entregue as provas e o roteiro para eu testar no Skip. Não marque a task como concluída sem meu teste humano explícito. Não implemente E4, multi-tenant nem autonomia de anúncios nesta retomada.

## E4 não está liberado por esta correção

O plano E4 de 29/10 é proposta de ingestão entre landing 67634 e sistema 51414, não SPEC aprovada. Sua menção às divergências documentais como bloqueio deve ser lida à luz da consolidação atual: essa parte documental foi corrigida, mas F3-T01 ainda precisa de implementação/prova/aceite e F3-T10 continua dependente dela. A ponte E4 ainda exige formalização própria (SPEC/task, identidade e idempotência, atribuição de campanha, contrato de campos, acesso, cadência e testes). Política LGPD aprovada continua obrigatória antes de usar dados pessoais reais; esta nota não relaxa esse gate. Não criar migrations, hooks ou ingestão com base apenas no plano.

## Limite desta entrega

Correção documental apenas. Nenhum código, migration, registro real, permissão, automação ou publicação de app foi alterado. Prazo 31/10 refere-se à cobrança documental, não ao aceite de todas as entregas seguintes.
