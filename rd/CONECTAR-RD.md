# Conectar RD Station em outra conta

Existem duas conexões independentes: **RD Station Marketing** e **RD Station CRM**. Ambas são remotas por HTTPS. O repositório inclui a configuração de exemplo e as instruções; o serviço é hospedado pela RD, não por este repositório.

## Gerar os links do novo gestor

1. Obter acesso próprio autorizado aos produtos RD do Spazio.
2. Entrar no [Catálogo MCP RD Station](https://mcp.rdstationmentor.com/) com essa conta.
3. Gerar o link pessoal do conector Marketing e o do CRM. Autorizar a conta correta em cada produto.
4. Guardar os links nas configurações privadas do assistente. Não colocá-los em arquivos do repositório, prompts, prints ou mensagens compartilhadas.

A RD informa que os links são individuais e descreve elegibilidade por produto/plano: Marketing Pro/Advanced e CRM Basic/Pro/Advanced. A disponibilidade precisa ser conferida na conta do cliente. [RD Station: configuração e requisitos](https://www.rdstation.com/inteligencia-artificial/mcp/), [Catálogo](https://mcp.rdstationmentor.com/).

## ChatGPT na web

No fluxo atual documentado: **Plugins → + → Add custom MCP server**. Criar uma conexão para cada produto, informar a respectiva URL gerada e seguir a autenticação indicada pelo Catálogo. Instalar o plugin resultante e selecioná-lo com `@` na conversa. A opção depende das permissões da conta/workspace. Interfaces anteriores podem mostrar **Settings → Apps → Create**. [Guia oficial](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt).

O formato configurado no ambiente de origem era:

```text
Marketing: https://mcp.rdstationmentor.com/marketing/mcp?key=CHAVE_PESSOAL_DO_NOVO_GESTOR
CRM:       https://mcp.rdstationmentor.com/crm/mcp?key=CHAVE_PESSOAL_DO_NOVO_GESTOR
```

São exemplos sem credenciais. Use os links completos que o Catálogo gerar, mesmo se o formato mudar. Não copiar a chave de um produto para o outro por suposição. Uma URL que contém `key` já carrega um segredo; não é um endpoint público sem autenticação.

## Codex

Mesclar o conteúdo de [config.codex.example.toml](config.codex.example.toml) na configuração privada local, substituindo cada URL pelo próprio link gerado. O exemplo restringe as ferramentas expostas no cliente às consultas observadas em 23/11/2026; essa lista não revoga permissões no servidor.

Também há [mcp.example.json](mcp.example.json) para clientes que usam `mcpServers`. Esse exemplo JSON declara transporte e URLs, sem lista de restrição de ferramentas; revisar as permissões no cliente antes de utilizá-lo.

Reiniciar ou abrir nova sessão conforme o cliente e conferir a lista de MCPs. Se o servidor exigir OAuth, completar o fluxo oferecido pelo cliente. [Configuração MCP](https://learn.chatgpt.com/docs/extend/mcp?surface=cli).

## Primeiro teste, sem alterar dados

```text
Teste os conectores RD CRM e RD Marketing usando somente consultas.
No CRM liste os funis, as etapas do PRINCIPAL e os campos personalizados.
No Marketing liste os campos de contatos.
Compare os IDs com rd/validacao-2026-11-23.json e rd/campos-crm.json.
Não crie contatos, não mova negociações e não inscreva leads em automações.
Informe se cada consulta respondeu e qual conta foi possível identificar.
```

Referência conferida em 23/11/2026: PRINCIPAL `6696bf6a4d06480016194374`; Em Negociação `678658c33003c600147b46b9`; Matrícula `6696bf6a4d06480016194379`.

## Se não conectar

| Situação | Próximo passo |
| --- | --- |
| Repositório abre, mas não existem ferramentas RD | Instalar/configurar os dois MCPs separadamente na conta nova |
| Catálogo não permite autorizar | Confirmar usuário, produto, conta selecionada e elegibilidade com o administrador RD |
| HTTP 401 | Refazer a autorização e gerar/atualizar o link pessoal; não reutilizar credenciais do antigo gestor |
| HTTP 403 | Conferir permissão ou plano; não repetir consultas sem corrigir a causa |
| HTTP 429 | Respeitar o intervalo indicado; as ferramentas deste ambiente recomendam pelo menos 60 segundos |
| Ferramentas existem, mas retornam outro funil/conta | Interromper o diagnóstico do Spazio e corrigir a conta autorizada |

Criar uma cópia deste repositório ou conectar o GitHub não instala automaticamente o MCP nem transfere a autorização RD.
