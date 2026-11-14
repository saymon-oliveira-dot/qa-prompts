# Tribo DotAI

Configuração dos boards da tribo DotAI. Use estes valores no passo de escopo em vez de perguntar projeto, filtro de squad e ligação. Pergunte só a squad (ou todas) e o período.

Site do Jira: `dot-group.atlassian.net`

## Boards

| Squad | Upstream | Downstream |
|---|---|---|
| DotSkills | `UP` (board 2696, unificado) | `DOTSKILLSD` (board 5888) |
| DotMind | `UP` (board 2696, unificado) | `DOTMINDD` (board 5887) |
| DotHub | `UP` (board 2696, unificado) | `INV` (board 1180) |

## Squad no upstream

O UP é um board só para as três squads. A squad vem do label e, quando falta label, do prefixo no título.

| Squad | Filtro no UP | Prefixo no título |
|---|---|---|
| DotSkills | `labels = DotSkills` | `[DOTSkills]` |
| DotMind | `labels = DotMind` | `[DOTmind]` |
| DotHub | `labels = DOTAI` | `[DOTai]` |

No UP, a DotHub aparece como DOTai. Não existe label `DotHub`.

Para pegar issues sem label, combine label e título: `(labels = DotSkills OR summary ~ "DOTSkills")`. Marque a issue que entrou só pelo título como "squad pelo título".

## Squad no downstream

O próprio projeto já é a squad. Não acrescente filtro de squad nos boards downstream.

O INV recebe também itens de Chat2flow e de atendimento (`[ATENDIMENTO]`). Eles passam pela regra de conteúdo funcional como qualquer issue.

## Ligação upstream → downstream

- Principal: link do tipo **Relacionadores** entre a issue do UP e a do downstream. Ex.: `UP-555` → `DOTSKILLSD-1077`, com o mesmo título.
- Quando não houver link: título idêntico no projeto downstream da squad. Marque o par como "par por título" e liste esses pares na saída para o usuário conferir.

## Status e tipos

- A categoria Done inclui o status **Cancelado**. Filtrar por nome de status no JQL não funciona neste Jira (devolve zero). Busque com `statusCategory = Done` e descarte do resultado as issues cujo status devolvido seja Cancelado. Pagine até ter 30 issues válidas por board ou acabar o resultado.
- Período: use `resolved >= -<DIAS>d`.
- No UP, a história é do tipo **Idéia**. As subtarefas (UX, pesquisa) ficam de fora: use `issuetype not in subTaskIssueTypes()`.
- No downstream, entram **História**, **Tarefa**, **Defeito** e **Alerta**. Subtarefas técnicas ("Frontend", "API", "Testes QA") ficam de fora pelo mesmo filtro.
