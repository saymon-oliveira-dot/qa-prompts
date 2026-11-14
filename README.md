# qa-prompts

Repositório das skills de QA da DotAI para o Cursor. Cada pasta na raiz é uma skill: um conjunto de instruções que ensina o agente a executar um fluxo de qualidade.

Uma skill não é código da aplicação. É o roteiro que o Cursor segue quando alguém pede aquele trabalho.

## Skills

| Pasta | O que faz |
|---|---|
| `qa-create-test-case` | Gera cenários de teste funcional a partir de um card do Jira (`PROJ-123`) ou de um texto colado (história, PRD ou critérios de aceite). Se a entrada for uma história e o conteúdo for suficiente, grava o resultado na subtarefa `[QA] Testabilidade e cenários`. |
| `qa-regression-scope` | Lê as histórias em Done dos boards upstream e downstream do Jira, cruza o que foi refinado com o que foi entregue e gera cenários regressivos por squad ou produto, no chat. Não grava nada no Jira. |

## Instalar na máquina

A instalação copia a pasta da skill para a pasta de skills do seu usuário. Depois disso ela vale em qualquer projeto aberto no Cursor, nesta máquina.

No PowerShell, na raiz deste repositório:

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.cursor\skills"
Copy-Item -Recurse -Force ".\qa-create-test-case" "$env:USERPROFILE\.cursor\skills\qa-create-test-case"
Copy-Item -Recurse -Force ".\qa-regression-scope" "$env:USERPROFILE\.cursor\skills\qa-regression-scope"
```

Copie só as linhas das skills que você quer usar.

O destino fica assim:

```text
C:\Users\<seu-usuario>\.cursor\skills\qa-create-test-case\
├── SKILL.md
└── references\
```

Copie a pasta inteira, inclusive `references`. Sem esses arquivos a skill perde o checklist, as heurísticas e o formato da saída.

Feche o chat atual e abra outro para o Cursor passar a usar a cópia nova. Para atualizar, rode o `Copy-Item` de novo.

Não copie a skill para `.cursor\skills-cursor`. Essa pasta é do Cursor.

## Usar só em um projeto

Se a skill deve existir apenas num repositório, copie a mesma pasta para `.cursor\skills\` desse repositório:

```text
<projeto>\.cursor\skills\qa-create-test-case\
```

Quem clonar o projeto e abrir no Cursor passa a ter a skill naquele repositório.

## Jira

Para ler o card e criar a subtarefa, o Cursor precisa do MCP Atlassian autenticado (Settings → MCP). Sem ele, a skill para e pede para habilitar o MCP. Ela não inventa o conteúdo do ticket.
