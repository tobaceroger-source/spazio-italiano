# Spazio Italiano — continuidade da gestão de tráfego

Pacote preparado em **23/11/2026** para o próximo gestor continuar a operação usando o próprio ChatGPT ou Codex, com prioridade para os MCPs **RD Station CRM** e **RD Station Marketing**.

## Comece por aqui

1. Leia [COMECAR-AQUI.md](COMECAR-AQUI.md).
2. Configure os dois MCPs seguindo [rd/CONECTAR-RD.md](rd/CONECTAR-RD.md).
3. Leia [PASSAGEM-DE-BASTAO.md](PASSAGEM-DE-BASTAO.md) e [rd/CRITERIOS-E-CONSULTAS.md](rd/CRITERIOS-E-CONSULTAS.md).
4. Cole o [PROMPT-DE-RETOMADA.md](PROMPT-DE-RETOMADA.md) no novo chat, com GitHub e os dois conectores RD selecionados.
5. Use [conversas/INDICE.md](conversas/INDICE.md) para aprofundar qualquer decisão.

**O repositório transporta contexto e configurações de exemplo. O acesso ao RD depende da autorização própria do novo gestor.** As URLs pessoais de conexão de Roger não foram incluídas.

## Conteúdo

| Pasta ou arquivo | Conteúdo |
| --- | --- |
| `rd/` | Guia de conexão; exemplos somente dos dois MCPs RD; catálogo de 83 ferramentas; critérios e validação de leitura |
| `conversas/` | Todas as 13 conversas de usuário localizadas no projeto local, inclusive uma arquivada; 308 turnos e 1.108 mensagens |
| `conversas/anexos-outros-clientes/` | Dois chats que estavam no projeto, mas tratam de SAZ e AJAX; não usar como contexto operacional do Spazio |
| `materiais/check-in/` | 12 registros de check-in, com lacunas e trechos incompletos preservados, além das orientações locais |
| `materiais/evidencias/` | Evidências locais de mídia, com período histórico identificado |
| `materiais/dashboard-spazio-replica/` | Fontes e instruções históricas de atualização do dashboard |
| `AGENTS.md` | Orientação de trabalho para um agente que abrir este repositório |
| `MANIFESTO.json` e `checksums.sha256` | Inventário e integridade dos arquivos publicados |

## Validação feita nesta entrega

Em 23/11/2026, consultas de leitura dos MCPs retornaram **12 funis CRM**, **12 etapas do funil PRINCIPAL**, **28 campos personalizados CRM** e **54 campos Marketing**. A fotografia está em [rd/validacao-2026-11-23.json](rd/validacao-2026-11-23.json) e [rd/campos-crm.json](rd/campos-crm.json).

Essas consultas validam o acesso da conexão de origem na data indicada. O novo gestor precisa repetir a validação com a própria conexão. Os resultados comerciais das conversas são históricos; não foram recalculados nesta entrega.

## Abrangência e limites

- Exportação textual de todas as páginas dos 13 chats de usuário encontrados no projeto **local do Codex**. Não é uma exportação global da conta ChatGPT.
- Foram omitidos raciocínio interno, instruções técnicas da plataforma, comandos e saídas brutas de ferramentas. Revisões automáticas e threads internas de subagentes não são chats de usuário e não entram na contagem.
- Imagens e arquivos anexados às mensagens permanecem como referências; seus binários não estão neste pacote. Também não foram copiados todos os vídeos, planilhas de leads, bases de dados ou projetos de código do computador.
- Os 20 materiais de apoio foram selecionados da pasta local do cliente. `materiais/manifesto.json` registra os caminhos de origem relativos.
- Senhas, tokens identificados, links pessoais de MCP e URLs secretas foram removidos do material. Não recuperar credenciais do histórico.
- Conectar o GitHub não concede acesso ao Google Drive, ao RD, às plataformas de anúncios ou à hospedagem. Cada serviço exige sua autorização própria.

Fontes públicas para a configuração: [MCP da RD Station](https://www.rdstation.com/inteligencia-artificial/mcp/), [conexão de MCP no ChatGPT](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt), [GitHub no ChatGPT](https://help.openai.com/en/articles/11145903-connecting-github-to-chatgpt).
