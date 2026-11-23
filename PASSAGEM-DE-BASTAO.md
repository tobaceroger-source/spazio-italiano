# Continuidade — Spazio Italiano

Preparação em 23/11/2026. O material reúne o que está registrado no projeto; não substitui a confirmação de decisões atuais com o cliente.

## Negócio, pessoas e frentes de mídia

Escola de italiano com operação presencial em Santo André e ofertas online. O histórico recente trabalha curso regular/presencial, preparação CILS presencial e online e Intensivo de Férias.

Nos check-ins aparecem Eduardo Kail (Dudu), relacionado ao atendimento/comercial do Spazio, Daniel Santos e Roger Tobace. Confirmar com o cliente os responsáveis atuais e as aprovações; participação nas reuniões não comprova cargo ou autorização vigente.

Em 09/11/2026 o plano relatado era **R$ 5.000 para novembro**: R$ 2.500 curso presencial, R$ 1.500 CILS e R$ 1.000 Intensivo. A conversa `App API da Meta` registra os orçamentos consultados, a divisão por frente e a revisão de criativos. São valores históricos, não instrução para reajustar campanhas hoje.

Na comparação de 09/11: CILS online atendia SP/PR/SC/RS; CILS presencial, cerca de 6,4 km da Rua Airó, 69, Santo André. O destino registrado era `https://lp.spazioitaliano.com.br/cils`. Havia recomendação de tornar a copy do online específica para essa modalidade. A entrega dos novos motions do Intensivo ainda precisava de confirmação após constarem em preparação. Fonte: [App API da Meta](conversas/app-api-da-meta--01a1c10f-f08c-7cc2-ae01-c1c32b9d01ec.md).

## Prioridade: RD CRM e Marketing

O CRM guarda negociações, funis, etapa atual, campos de origem, situação e fechamento. O Marketing complementa os eventos de conversão e UTMs dos formulários nativos. Só consultar o CRM pode perder a atribuição de candidatos que chegam sem `FONTE_2 = V4`.

O acesso de origem foi testado em 23/11/2026: listagem de funis, etapas e campos respondeu. Nenhum indicador comercial foi recalculado. O novo gestor deve gerar sua própria conexão conforme [rd/CONECTAR-RD.md](rd/CONECTAR-RD.md).

Referências conferidas:

| Recurso | Identificador |
| --- | --- |
| Funil PRINCIPAL | `6696bf6a4d06480016194374` |
| Etapa Em Negociação | `678658c33003c600147b46b9` |
| Etapa Matrícula | `6696bf6a4d06480016194379` |
| Campo CRM FONTE_2 | `6696c716c591b90021a88dd8` |

O roteiro e as diferenças de universo estão em [rd/CRITERIOS-E-CONSULTAS.md](rd/CRITERIOS-E-CONSULTAS.md).

## Últimos registros comerciais, com suas datas

Na conversa RD, a atualização da planilha de **01 a 19/11/2026** foi relatada e conferida com **55 SQL Meta e 13 SQL Google**, distribuídos pela criação do lead, considerando a etapa atual Em Negociação. Outros três registros sem UTM atribuível ficaram fora dessas duas colunas. Esses números são um ponto de reconciliação do histórico; não são um resultado atual de 23/11.

No mesmo dia, a conversa relatou uma negociação ganha em novembro no funil Convênios, sem atribuição V4 e com valor registrado de R$ 0,00, e nenhuma venda V4 identificada no período. Ausência de venda sinalizada ou valor zerado no CRM não confirma ausência de receita fora dele.

Em 19/11 foi registrada uma correção da fórmula MQL da origem da planilha: objetivo contendo viagem, viajar ou cidadania retorna `sim`; para os demais, permanece a regra de investimento. Essa regra não substitui o critério do relatório RD em que MQL equivale a todos os leads V4 do recorte.

Fonte: [MCP RD STATION](conversas/mcp-rd-station--01a0d07f-c570-7f91-a5fa-f8b50055e059.md). As afirmações são preservadas conforme o registro da conversa, sem nova auditoria da planilha ou da base comercial nesta entrega.

## Ativos e acessos úteis

| Ativo | Referência e dependência |
| --- | --- |
| Repositório de continuidade | https://github.com/tobaceroger-source/spazio-italiano — liberar acesso GitHub ao novo gestor |
| Planilha de resultados | https://docs.google.com/spreadsheets/d/1ODujKJqKFMGm8sXH9dzNdnLD9i2gAkE9N3PsA_G8fCg/edit — requer compartilhamento Google |
| Origem da aba de leads | https://docs.google.com/spreadsheets/d/1WpcXQNxRHM6TlC_YYD9UuMnv0vliYqSs_QXxZ15u0os/edit — requer compartilhamento Google |
| Dashboard | https://spazio-italiano-dashboard.rogertobace.chatgpt.site — a gestão da hospedagem é um acesso separado |
| Google Ads | Conta `7157090272`, historicamente via Canovas Mendes — não conectado por este pacote RD |
| Meta Ads Spazio | Conta `1287402738480607` — não conectado por este pacote RD |
| Criativos de novembro citados | https://drive.google.com/drive/folders/1Xjis2dCfFvU1IPfYV3LNaYfYuQPHgzeA — compartilhar pela conta do cliente |
| Backup de sites e código | https://github.com/tobaceroger-source/sites- — acesso privado separado; procurar os projetos Spazio no catálogo |

Não depender do e-mail corporativo antigo de Roger para manter os acessos do próximo gestor. O repositório não transfere propriedade de documentos, hospedagem, anúncios, integrações ou automações.

## Pendências que exigem retomada

| Tema | Último estado documentado | Próxima ação |
| --- | --- | --- |
| Credenciais RD do novo gestor | Ainda dependem da conta de destino | Gerar links pessoais e repetir consultas de validação |
| Critérios de SQL | Planilha, bloco RD e funil de mídia têm definições distintas | Selecionar o relatório e manter os universos separados |
| Formulários nativos e FONTE_2 | Há histórico de negociações sem marca V4 e proposta de correção via Make | Confirmar fluxo atual; não criar negociações duplicadas para preencher origem |
| UTMs e formulários | Recuperação no Marketing foi necessária; nome antigo de campanha pode continuar recebendo leads novos | Conferir evento e data reais, não inferir mês pelo nome do formulário |
| Dashboard | Até o último registro de 19/11, escrita impedida por falta do token específico | Obter o acesso de escrita por canal autorizado; não desproteger nem republicar como atalho |
| Atualidade do dashboard | Último registro: carga geral até 31/10; bloco RD até 21/10 | Ler a fonte atual antes de reutilizar os números |
| Mensuração servidor | Em 27/10 houve publicação da filtragem de nome/e-mail/telefone do GA4; validação do servidor ficou pendente por HTTPS/404 | Conferir situação atual com o responsável técnico |
| Google Ads | Disparo direto da tag foi testado; contabilização e percurso via servidor não ficaram integralmente confirmados | Verificar conversões usadas nas campanhas atuais |
| Intensivo e CILS | Configurações e preparação de anúncios relatadas em 09/11 | Reconsultar entrega, gasto e oferta vigente antes de ajustar verba |
| Check-ins | Há 12 registros locais, alguns incompletos ou só marcadores de ausência | Priorizar decisões recentes e validar pendências com o cliente |

Fontes das pendências: [DASHBOARD](conversas/dashboard--01a0f89b-5aea-7280-995b-d049d4ee1a9e.md), [MCP RD STATION](conversas/mcp-rd-station--01a0d07f-c570-7f91-a5fa-f8b50055e059.md), [Subir criativos na Meta Ads](conversas/subir-criativos-na-meta-ads--01a10e95-e80c-7743-ba10-909a0c4c1181.md) e [check-ins](materiais/check-in/CONTEXTO-RETOMADA.md).

## Sequência recomendada para a entrada

1. Confirmar acesso ao repositório, aos dois RD e às planilhas.
2. Validar a conta RD por funis, etapas e campos; registrar data da conferência.
3. Reproduzir primeiro o relatório comercial do mês com o critério explícito, mantendo atribuídos e sem origem separados.
4. Conciliar o resultado com a planilha e investigar divergências sem sobrescrever dados históricos.
5. Confirmar com o cliente ofertas, verba, turmas e atendimento; depois priorizar mudanças de mídia.
6. Retomar a atualização do dashboard somente quando as fontes e o acesso de escrita estiverem disponíveis.
