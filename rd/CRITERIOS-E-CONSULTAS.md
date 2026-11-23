# Como consultar RD sem misturar os indicadores

Este arquivo reconcilia as definições registradas nas conversas. A confirmação de conexões em 23/11/2026 não recalculou os resultados comerciais.

## Três universos diferentes

| Relatório | Universo e atribuição | Significado de SQL/oportunidade |
| --- | --- | --- |
| Planilha Métricas Comerciais — critério reafirmado em 19/11 | Leads V4 do PRINCIPAL; canal por UTM do CRM ou evento Marketing; dia da criação | Estão **atualmente** em Em Negociação |
| Último bloco RD do dashboard | Histórico aprovado considera todos os funis/status; `FONTE_2=V4` e formulários nativos V4 comprovados por evento/UTM; deduplicar `deal.id` | Subconjunto **atualmente** em Em Negociação; não histórico de passagem |
| Funil de mídia do dashboard | Todas as oportunidades criadas no PRINCIPAL; plataforma somente por UTMs; sem filtro de etapa/status | Todas as oportunidades criadas no recorte, mesmo que a etapa atual seja outra |

Não somar ou comparar esses três totais como se tivessem a mesma definição. Perguntar qual deles reproduzir antes de alterar um relatório.

Há uma ambiguidade no próprio histórico: em 18/11 o relatório ampliou o universo com nativos V4 sem marca no CRM; em 19/11 a confirmação do preenchimento descreveu `FONTE_2=V4`. Para preservar rastreabilidade, devolver separadamente: **A — marca V4 no CRM; B — nativo confirmado por evento/UTM sem marca; C — candidato sem prova**. Explicar se o total inclui A, A+B ou outro universo acordado. Não inventar que essa reconciliação já foi feita.

## MQL tem dois usos

- Relatório RD: o usuário definiu MQL como todos os leads V4 do universo escolhido, sem a qualificação de investimento aplicada na planilha.
- Campo MQL da planilha: fórmula própria. A correção registrada em 19/11 dá prioridade a viagem/viajar/cidadania para retornar `sim`; os demais objetivos seguem a resposta de investimento.

## Ferramentas e campos

O prefixo varia por cliente. Os nomes abaixo foram observados neste ambiente; descobrir os equivalentes na conexão atual.

| Necessidade | Ferramentas de consulta |
| --- | --- |
| Encontrar funis/etapas | CRM `funnel_list`, `funnel_stages_list` |
| Descobrir campos reais | CRM `custom_fields_list`; Marketing `contact_custom_fields_list` |
| Consultar negociações e situação | CRM `deals_list`, `deals_get` |
| Relacionar contato | CRM `contacts_list`, `contacts_get`; Marketing `get_contact_by_id_email` |
| Recuperar evento e UTM | Marketing `get_contact_events`, com `event_type=CONVERSION` e UUID do contato |
| Ver formulários/campanhas | Marketing `forms_search`, `campaigns_search` |

CRM confirmado em 23/11/2026:

| Campo | ID |
| --- | --- |
| FONTE_2 | `6696c716c591b90021a88dd8` |
| utm_campaign | `69efeeed45cba30013b292dd` |
| utm_source | `69efef026ce34d00199c4e79` |
| utm_medium | `69efef338151770013008992` |
| utm_content | `69efef12f8c9220024ea851a` |
| SOURCE | `6a0c9143e95c850014e92a86` |
| CAMPAING — grafia existente | `6a0caaab32470200138e9566` |
| MEDIUM | `6a0caab86df7d400149555b8` |
| CONTENT | `6a0caac7916f3f001570dd67` |

Não inventar um `@slug` de RDQL a partir do nome do campo: consultar os metadados atuais. Se o filtro por campo não estiver disponível, aplicar a seleção à resposta completa, com paginação e validação.

## Roteiro para reproduzir SQL da planilha

1. Explicitar início/fim, cobertura e fuso `America/Sao_Paulo`. Ao agrupar, usar a criação da negociação/lead conforme o critério do relatório; não usar a data da consulta como data de geração.
2. Validar PRINCIPAL `6696bf6a4d06480016194374` e Em Negociação `678658c33003c600147b46b9` na conta atual.
3. Consultar negociações criadas no recorte e validar a etapa atual. Percorrer **todas** as páginas. Deduplicar `deal.id`; isso conta negociações, não necessariamente pessoas únicas.
4. Separar marca `FONTE_2=V4`, nativos V4 com prova e candidatos, conforme a ambiguidade histórica acima.
5. Atribuir Google/Meta somente com UTM válida. Se faltar, consultar o evento correspondente no Marketing. Não inferir V4 por pertencer a todo o grupo META.
6. Usar a data e os dados do evento correto. Uma campanha chamada “Intensivo de Férias — Julho” ou “CURSO IF 2026 03/11/2025...” pode receber conversões em outro mês.
7. Manter sem origem identificada em linha própria. Não distribuir proporcionalmente nem completar por suposição.
8. Conferir somas por dia, canal e campanha. Registrar se o último dia está parcial e se alguma parte depende de revisão.

Exemplo de filtro RDQL por funil e etapa, **sem recorte de data**:

```json
{
  "filter": "pipeline_id:6696bf6a4d06480016194374 stage_id:678658c33003c600147b46b9",
  "page": {"number": 1, "size": 100}
}
```

O filtro acima é ilustrativo e não foi executado nesta entrega. Para um relatório mensal, acrescentar os limites de criação compatíveis com o esquema atual e validar os registros retornados no fuso acordado. O parâmetro recebe a expressão RDQL, sem escrever `filter=` dentro da string.

Exemplo de chamada para eventos Marketing, após descobrir o UUID real:

```json
{"uuid": "UUID_REAL_DO_CONTATO", "event_type": "CONVERSION"}
```

O catálogo [catalogo-ferramentas.json](catalogo-ferramentas.json) preserva as descrições e declarações de argumentos observadas. Não contém credenciais nem substitui a descoberta das ferramentas atuais.

## Vendas e comparações

- Vendas do período: separar data de fechamento (`closed_at`), situação ganha e atribuição de origem. Não restringir automaticamente a leads criados no mesmo mês.
- Estar em Matrícula e estar com situação ganha são verificações diferentes. Não modificar a situação para fazê-las coincidir sem uma solicitação atual.
- Receita zero pode significar ausência de valor preenchido. Não concluir ausência de faturamento apenas desse dado.
- SQL atual pode diminuir se a negociação avançar ou for encerrada. Essa mudança não significa necessariamente perda de histórico ou falha na extração.
- Para comparar com resultados de plataforma, alinhar período, evento, objetivo e janela de atribuição. Formulário nativo e lead no site não devem ser somados indiscriminadamente para toda campanha.

## Histórico de integração a continuar

Houve identificação de criação nativa de negociação sem `FONTE_2=V4`, especialmente no fluxo Viagem. Foi proposta uma atualização da negociação existente pelo Make. O material não comprova implantação definitiva dessa correção; confirmar o cenário atual e o responsável antes de tocar na integração. Criar outra negociação só para incluir origem pode duplicar a contagem.

## Modelo de resposta

```text
Período e cobertura:
Conta e funil consultados:
Definição de MQL e SQL utilizada:
Universo A / B / C e quais entram no total:
Google:
Meta:
Sem origem:
Total e conferência das somas:
Comparação com a planilha:
Limitações e pendências:
```

Fonte operacional: conversas `MCP RD STATION`, `DASHBOARD` e `PLANILHAS DE LEAD`, listadas em [INDICE](../conversas/INDICE.md). Metadados atuais: [validacao-2026-11-23.json](validacao-2026-11-23.json) e [campos-crm.json](campos-crm.json).
