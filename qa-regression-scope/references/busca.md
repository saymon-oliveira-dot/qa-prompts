# Busca no Jira

## Resolver o campo de squad ou produto

- **Componente ou label:** use o nome informado pelo usuário direto no JQL.
- **Campo personalizado:** descubra o ID do campo pelo MCP Atlassian (use `discover` com um objetivo como "listar campos do Jira" e rode a operação devolvida com `executeRead`). Use o ID ou o nome exato devolvido. Não adivinhe `customfield_*`.
- Se o campo não for encontrado, informe o nome que o usuário passou e pergunte qual é o campo certo. Pare até ter a resposta.

## Modelos de JQL

Troque os valores entre `< >` pelo que o usuário informou ou pela configuração da tribo. Não acrescente filtros que ninguém pediu.

Upstream:

```text
project = <UPSTREAM> AND issuetype not in subTaskIssueTypes() AND statusCategory = Done AND <FILTRO_SQUAD_PRODUTO> AND resolved >= -<DIAS>d ORDER BY resolved DESC
```

Downstream:

```text
project = <DOWNSTREAM> AND issuetype not in subTaskIssueTypes() AND statusCategory = Done AND resolved >= -<DIAS>d ORDER BY resolved DESC
```

Acrescente `<FILTRO_SQUAD_PRODUTO>` no downstream só quando o projeto downstream for compartilhado entre squads.

Exemplos de `<FILTRO_SQUAD_PRODUTO>`:

- componente: `component = "Pagamentos"`
- label: `labels = DotSkills`
- campo personalizado: `"Squad" = "Pagamentos"`

Quando houver uma história ou épica de foco, acrescente o filtro que o usuário confirmar (ex.: `"Epic Link" = PROJ-10` ou `issue in linkedIssues(PROJ-123)`).

## Cancelados

A categoria Done costuma incluir status de cancelamento. Descarte do resultado toda issue cujo status devolvido indique cancelamento (ex.: Cancelado). Não conte a issue descartada no limite.

## Limites

- Considere no máximo 30 issues válidas por board, as resolvidas mais recentemente. Pagine até chegar nesse número ou acabar o resultado.
- Se houver mais issues válidas que o limite, informe no chat e sugira reduzir o período ou separar por squad.
- Peça os campos summary, description, status, issuetype, resolutiondate, issuelinks, parent, components, labels e o campo de squad/produto. Leia comentários só das issues que entrarem em pares.

## O que mostrar no chat antes de seguir

- O JQL usado em cada board.
- Quantas issues cada busca devolveu e quantas foram descartadas como canceladas.
