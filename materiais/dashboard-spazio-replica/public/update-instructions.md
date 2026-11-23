# Atualização do dashboard Spazio Italiano

Site: appgprj_6ad0fccb18148191b74ef18abf088667. A consulta é pública. As duas rotas PUT exigem `X-Dashboard-Write-Token`, validado contra o segredo `DASHBOARD_WRITE_TOKEN` do runtime. Entregar a credencial de gravação aos scripts no campo `writeToken` via stdin. Acesso de serviço do Sites não substitui essa credencial. Se a execução não puder obtê-la por um canal seguro, preservar o painel e avisar. Nunca guardar credenciais no código, no prompt ou nos arquivos de carga.
Atualizar a base persistente; não republicar código nas cargas diárias. Consultar o Site para obter acesso de serviço e enviar o bearer somente ao próprio Site no cabeçalho OAI-Sites-Authorization. Esse acesso não fornece consentimento para os conectores.

## Fontes e contrato

Planilha 1ODujKJqKFMGm8sXH9dzNdnLD9i2gAkE9N3PsA_G8fCg. Ler valores UNFORMATTED_VALUE, preservando cabeçalho e posição de linhas. Conferir metadados para os limites atuais antes da leitura. Intervalos iniciais:
- meta = 'Meta Ads - Plataforma'!A1:U6290
- metaLead = 'Meta Ads - Plataforma'!X1:Y6290 (mesmas linhas de meta)
- google = 'Google Ads'!A1:O1000
- commercial = 'Métricas Comerciais'!A1:N1000

Google Ads: conta 7157090272, Spazio Italiano, BRL, America/Sao_Paulo. Usar a conexão Canovas Mendes, leitura somente. Conferir a API com customer, segments.date, metrics.cost_micros, metrics.clicks, metrics.impressions e metrics.conversions, após consultar metadados. Período de conferência: últimos 45 dias completos, até ontem em São Paulo. Segmentar conversion_action_name para confirmar que são formulários. Não contar outros tipos de conversão como Leads. googleApi é o array result com nomes de campo completos. As linhas da API substituem mídia/Leads da planilha nessas datas; cost_micros / 1e6.

Meta: conta 1287402738480607. Conferir is_queryable e metadados dos campos antes de consultar. Comparar a soma de X+Y com os resultados de formulários nativos (actions:leadgen.other) e LP (actions:offsite_conversion.fb_pixel_lead), por campanha/dia no mesmo período. Não usar o campo genérico lead como total corrigido: em 01–14/10 ele retornou 269 contra 102 formulários/LP. Não somar abas de diferentes detalhamentos. metaVerified é o array ad_entities retornado pelo MCP com id,date_start,results. Em 01–14/10: formulários nativos 57, LP 45. Os nomes das colunas X/Y na planilha aparecem trocados em relação às categorias da API, mas sua soma coincide. Cliques são Cliques no link (K), não todos os cliques. Alcance/frequência ficam sem total porque não são aditivos.

RD: SQL significa TODAS as oportunidades criadas no funil PRINCIPAL 6696bf6a4d06480016194374, sem filtro de etapa ou status. A regra anterior de passagem por Em Negociação foi substituída pelo usuário em 15/10/2026. Contar IDs únicos por data de criação em America/Sao_Paulo. Usar limites explícitos com offset -03:00 e paginar até o fim. Não usar SQL da planilha.

Separar por utm_source; na ausência, SOURCE (slug source), acompanhado de utm_medium/MEDIUM e utm_campaign/CAMPAING. UTMs têm prioridade sobre source_id antigo. Google pago: google/googleads/google_ads/adwords com evidência de campanha paga. Meta: facebook/fb/instagram/ig/meta com evidência de campanha paga. A campanha RD 6a0cc7fda3691d0001e49ab4 é V4 - LP. Campanhas [Vxx] e medium cpc/paid/ppc/paid_social também confirmam mídia paga. Não converter Google orgânico em Google Ads.

Separação estritamente por UTM, conforme escolha explícita do usuário. Não preencher Google/Meta por source_id, FONTE_2 ou nome da campanha RD quando UTM_SOURCE/SOURCE estiver ausente. sql.sourceFallback=false. Sem UTM = other. UTMs têm prioridade sobre origens legadas; não transformar orgânico em mídia paga. Usar lib/rd-sql.mjs: aggregateOpportunities(deals,{from,through}) gera rd agregado; applySQL(snapshot,rd) aplica aos registros. Nunca enviar dados pessoais ou IDs individuais ao site.

A carga inicial cobre 01–14/10 no horário de São Paulo: 175 oportunidades; 34 Meta + 22 Google por UTM; 119 sem UTM. SQL do funil de mídia = 56. A automação deve preservar a cobertura desde 01/10 e expandir até ontem, relendo o mês inteiro para capturar mudanças de atribuição. Fora da cobertura SQL, manter null. Negócios ganhos e perdidos continuam contando como oportunidades criadas.

## Gravação e verificação

GET /data.json lê a base atual. PUT /api/snapshot recebe JSON:
{meta:[linhas],metaLead:[linhas],google:[linhas],commercial:[linhas],googleApi:[linhas API],metaVerified:[linhas API],rd:{from,through,checkedAt,total,sourceFallback,records:[{date,channel,method,count}]},updatedAt:"ISO UTC",dataThrough:"YYYY-MM-DD de ontem"}.

O servidor normaliza por data/canal e persiste no R2; faltas ficam null. MQL/vendas/faturamento vêm exclusivamente das colunas B/D/E (Meta) e F/H/I (Google) de Métricas Comerciais. Datas futuras são excluídas. Orçamento mensal permanece null enquanto não houver meta confirmada. Gênero/idade aguardam detalhamento dos Leads corrigidos; plataforma usa os mesmos X+Y.

Só gravar após leitura completa das quatro faixas e conferência das APIs. Se uma fonte falhar, preservar a base anterior e relatar a falha; não atualizar updatedAt nem converter ausência em zero. Repetir falhas transitórias uma vez, respeitando limites; autenticação exige reconexão, não loop de tentativas. Conferir dias com gasto sem Leads: zero só após confirmação de ausência de conversões, nunca só por falta de linha.

Após PUT, GET /data.json com no-store e conferir updatedAt, cobertura, registros e totais de cada canal. Recusar datas duplicadas e métricas negativas/não finitas. Rotina silenciosa quando saudável; avisar somente falha, divergência relevante ou ação necessária.


## Configuração do painel aprovada em 15/10

Manter os blocos Indicadores complementares e Fontes e conferência removidos. Preservar lib/dashboard-settings.mjs: meta de mídia de outubro R$ 5.000, rateada proporcionalmente ao gasto do mês de Meta e Google. Para outubro encerrado, a aba Métricas Comerciais substitui a correção parcial de 01–14/10. Usar as datas e valores por canal da planilha. O servidor reaplica a configuração em cada gravação.

Atualizar todas as fontes seguintes e enviar uma carga completa. O writer recusa cargas sem campanhas, contatos RD ou frequência. Pode enviar as fontes raw ao endpoint, usando os nomes abaixo, ou rodar enrich(snapshot,input) de lib/enrich.mjs localmente e enviar o resultado. Não publicar dados pessoais; contatos e negociações usados no cruzamento devem ter só IDs internos, datas, vínculo de contato, pipeline e campos UTM. O R2 persiste somente agregados.

- gender = 'Meta Ads - Genero'!A1:G6290; age = 'Meta Ads - Idade'!A1:G8422 (atualizar limites por metadados). Somar G por A/E. São eventos de Lead reportados nas abas, não os 102 Leads corrigidos. Não forçar igualdade nem substituir os Leads do funil. A UI identifica essa diferença.
- metaPeriod = array de account insights de Meta, uma consulta única de from a through, com reach,frequency,impressions,video_play_actions e marcos de vídeo. Alcance/frequência não são somáveis. Só exibir o valor quando o filtro corresponder à consulta completa.
- videoDaily = account insights diários, com date_start, video_play_actions, video_p25_watched_actions, video_p50_watched_actions, video_p75_watched_actions, video_p100_watched_actions. Retenção = soma de cada marco / soma de reproduções iniciadas.
- metaCampaigns = insights diários das campanhas com id,name,amount_spent,impressions,link_click,results; Leads corrigidos vêm somente de results indicator actions:leadgen.other ou actions:offsite_conversion.fb_pixel_lead. Conferir completude das campanhas ativas no período; não limitar silenciosamente a lista. No período inicial os IDs são 120252292345680603,120246937657010603,120246010948960603,120244867137620603.
- googleCampaigns = resultados diários da API Google Ads para campaign.id,name,url_custom_parameters e métricas conversions,cost_micros,clicks,impressions,segments.date. Conferir ações de conversão para manter somente formulários. Os parâmetros de URL ajudam a vincular UTM campaign por correspondência confirmada.
- meta,metaLead,google = as faixas históricas já descritas; lib/enrich.mjs também monta o histórico das tabelas de campanhas e substitui o período conferido pelas APIs.
- contactsDaily = [{date:'YYYY-MM-DD',contacts:[{id}]}], todos os contatos criados no CRM, de from a through. A paginação ampla de contacts_list mostrou resultados instáveis em 15/10: usar intervalos de um dia em São Paulo, page size 200 (máximo), conferir ausência de next; se houver next, dividir em intervalos menores para cobrir tudo sem sobreposição. Deduplicar IDs. Não usar contagem de oportunidades como Leads. A leitura diária completa inicial confirmou 208 IDs únicos.
- deals = todas as negociações criadas no mesmo período, em TODOS os funis, paginadas integralmente; campos mínimos id,custom_fields UTM/SOURCE/MEDIUM/CAMPAING e details.pipeline_id,created_at,contact_ids,campaign_id. Contatos podem ter múltiplas negociações; usar somente a primeira UTM preenchida das negociações vinculadas para cada contato. SQL continua contando apenas o PRINCIPAL, 6696bf6a4d06480016194374. Todas as etapas e status participam.
- from e through = janela completa consultada no RD/contatos/APIs. A cobertura inicial é 01–14/10. Expandir a janela até ontem. Não marcar como zero datas não consultadas.

Tabelas de campanhas: preservar nomenclaturas das plataformas; vincular RD somente por igualdade normalizada do nome, ID ou parâmetro de campanha efetivamente consultado. Não presumir que pesquisa-online = online, nem agrupar campanhas antigas só por código [V01]. Quando a correspondência não estiver comprovada, manter em UTMs com outras nomenclaturas e exibir a UTM literal no bloco Leads RD — UTMs. Campo source_id e FONTE_2 nunca completam UTMs ausentes.

A carga inicial apurou 208 contatos: Meta 36, Google 24 e sem UTM 148; 175 oportunidades no PRINCIPAL: Meta 34, Google 22 e sem UTM 119. Leads de mídia são uma métrica diferente. Data pessoal nunca aparece nas tabelas.

Consultar metaPeriod também para o mês corrente, atribuindo from/through exatos a cada resultado; pode conter várias janelas consultadas. videoDaily define videoCoverage: nunca calcular retenção para datas sem denominador de reproduções iniciadas.


## Último bloco RD: atualização diária aprovada em 15/10/2026

Esta seção substitui a instrução anterior de manter o último bloco fixo. Atualizar o bloco todos os dias às 8h America/Sao_Paulo, pelo RD CRM + RD Marketing, com o MESMO recorte da conversa MCP RD STATION. O restante do funil mantém as regras anteriores.

Recorte: negociações/leads V4 criados no mês, em TODOS os funis e status; cada deal.id uma vez. Não contar contatos CRM como unidade desse bloco. Incluir Fonte 2 = V4 e também leads com conversão comprovada em formulário nativo V4, mesmo com Fonte 2 Grupo META/DESCONHECIDO. Nunca atribuir Grupo META inteiro. MQL = todos os leads desse recorte. SQL = subconjunto desses mesmos leads cujo stage_id ATUAL é 678658c33003c600147b46b9 (Em Negociação, PRINCIPAL), sem histórico de passagem e sem usar created_at da mudança de etapa. Reconsultar todo o mês para capturar saídas da etapa; SQL pode diminuir.

Janela diária: do dia 1 do mês até ontem, dias completos em São Paulo; no primeiro dia do mês, concluir o mês anterior. Usar limites datetime explícitos -03:00, nunca datas UTC implícitas. Ler deals_list em todos os funis com created_at >= início e < dia seguinte ao fim, page size 200, seguir next até acabar; deduplicar IDs e confirmar completude. Conferir funnel_stages_list do PRINCIPAL. Ler campaigns_list para nomes/IDs de campanhas nativas. IDs confirmados: 6aced648caf04000012842e5 = V4 - FORMULARIO NATIVO - VIAGEM → [V4] CURSO VIAGEM; 6a0cdc91d5d8870001363421 = V4 - FORMULARIO NATIVO - PRESENCIAL → [V4] CURSO PRESENCIAL. Para campanhas novas, confirmar nome exato na API antes de adicionar nativeCampaigns. Não confundir Italiano Viagem 2026 (69827afe655e250001fc85d2) com prova de V4: esse fluxo também tem origem orgânica.

Plataforma/campanha: UTM_SOURCE/SOURCE identifica Google (google/googleads/google_ads/adwords) ou Meta (meta/facebook/fb/instagram/ig/ig/fb); campanha vem de UTM_CAMPAIGN/CAMPAING, preservando nomenclatura. Para formulários nativos comprovados, plataforma Meta e campanha Formulário [V4] CURSO VIAGEM/PRESENCIAL. Sem comprovação de plataforma, Sem origem identificada; não inventar UTMs. Remover apenas o prefixo visual 🟢 C - dos nomes. Exibir campanhas novas automaticamente; somas de campanhas devem bater com resumo/total e SQL <= MQL.

Para identificar formulários antigos: consultar contacts_get dos contact_ids vinculados, usar email/phone APENAS transitoriamente para get_contact_by_id_email no Marketing e get_contact_events(CONVERSION); confirmar event_identifier EXATO [V4] CURSO VIAGEM ou [V4] CURSO PRESENCIAL dentro da janela mensal. Guardar somente {contactId,uuid,checkedAt,events:[{identifier,timestamp}]} em evidence/rd-form-evidence.json, ignorado pelo Git. O cache inicial tem 40 contatos auditados; reaproveitar provas históricas e consultar contatos novos ou sem prova conclusiva para reduzir custo. Um 404 confirmado no Marketing é ausência de vínculo, não comprovação de origem. Falha de permissão/autenticação/rate limit não é zero nem orgânico: preservar o relatório anterior e avisar. Respeitar 120 requisições/minuto; lotes de no máximo 20 por vez e 60s de espera se houver 429, apenas uma repetição transitória.

Gerar relatório agregado com aggregateRDAnalysis(deals,{from,through,checkedAt,formEvidence,nativeCampaigns}) em lib/rd-analysis.mjs. Validar com validateRDAnalysis. Não publicar IDs individuais, email, telefone, nome de pessoa ou eventos brutos. Salvar somente o relatório agregado em arquivo local ignorado e enviar para PUT /api/rd-analysis (objeto do relatório direto, não wrapper). Endpoint separado preserva o restante da base e a gravação geral /api/snapshot não apaga o relatório. GET /data.json retorna rdAnalysis. Use scripts/update-rd-analysis.mjs em PTY; aguarde Ready for RD service JSON, envie stdin oculto {token,path} usando siwc_bypass_bearer_token fresco de get_site do próprio projeto. Nunca imprimir/salvar token. Conferir readback checkedAt, datas, summary, campaigns e totals. Sem republicar código nas atualizações.

Referência de conferência em 15/10 às 15h06: 127 MQL (Meta 97, Google 26, sem origem 4), SQL atual 50 (Meta 33, Google 17), após mudança de etapa de um lead Meta; não fixar os 51 SQL da análise das 13h. Essa referência incluiu o dia 15 em andamento; as próximas cargas agendadas usam dias completos.

O botão Mês atual deve terminar na última data disponível da base de mídia, evitando esconder SQL ao incluir um dia ainda não consultado. O último bloco RD tem janela e horário próprios exibidos na tela, independentemente do filtro da mídia.
