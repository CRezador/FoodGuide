# Rastreador de issues: Markdown local

As issues e specs deste repositório vivem como arquivos markdown em `.scratch/`.

Este repositório não tem remote git configurado, portanto não existe rastreador de issues no GitHub/GitLab para chamar. **Não** execute comandos `gh issue` ou `glab issue` aqui.

## Convenções

- Uma feature por diretório: `.scratch/<feature-slug>/`
- A spec fica em `.scratch/<feature-slug>/spec.md`
- As issues de implementação são um arquivo por ticket em `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numeradas a partir de `01` — nunca um único arquivo combinado de tickets
- O estado de triagem é registrado como uma linha `Status:` no topo de cada arquivo de issue (veja `triage-labels.md` para as strings de cada papel)
- Comentários e histórico de conversa são adicionados ao final do arquivo, sob um cabeçalho `## Comments`

## Quando uma skill disser "publique no rastreador de issues"

Crie um novo arquivo em `.scratch/<feature-slug>/` (criando o diretório, se necessário).

## Quando uma skill disser "busque o ticket relevante"

Leia o arquivo no caminho referenciado. Normalmente o usuário vai passar o caminho ou o número da issue diretamente.

## Operações de wayfinding

Usadas por `/wayfinder`. O **mapa** é um arquivo com um arquivo **filho** por ticket.

- **Mapa**: `.scratch/<effort>/map.md` (o corpo com Notas / Decisões-até-agora / Névoa).
- **Ticket filho**: `.scratch/<effort>/issues/NN-<slug>.md`, numerado a partir de `01`, com a pergunta no corpo. Uma linha `Type:` registra o tipo do ticket (`research`/`prototype`/`grilling`/`task`); uma linha `Status:` registra `claimed`/`resolved`.
- **Bloqueio**: uma linha `Blocked by: NN, NN` no topo. Um ticket está desbloqueado quando todos os arquivos que ele lista estão `resolved`.
- **Fronteira**: varra `.scratch/<effort>/issues/` buscando arquivos abertos, desbloqueados e não reivindicados; o menor número vence.
- **Reivindicar**: defina `Status: claimed` e salve antes de qualquer trabalho.
- **Resolver**: acrescente a resposta sob um cabeçalho `## Answer`, defina `Status: resolved` e então adicione um ponteiro de contexto (resumo + link) às Decisões-até-agora no `map.md`.
