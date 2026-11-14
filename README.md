# qa-prompts

Repositório das skills de QA da DotAI para o Cursor. Cada pasta na raiz é uma skill: um conjunto de instruções que ensina o agente a executar um fluxo de qualidade.

Uma skill não é código da aplicação. É o roteiro que o Cursor segue quando alguém pede aquele trabalho.

## Skills

| Pasta | Versão | O que faz |
|---|---|---|
| `qa-create-test-case` | 2.0 | Gera cenários de teste funcional a partir de um card do Jira (`PROJ-123`) ou de um texto colado (história, PRD ou critérios de aceite). Se a entrada for uma história e o conteúdo for suficiente, grava o resultado na subtarefa `[QA] Testabilidade e cenários`. |
| `qa-regression-scope` | 1.0 | Lê as histórias em Done dos boards upstream e downstream do Jira, cruza o que foi refinado com o que foi entregue e gera cenários regressivos por squad ou produto, no chat. Não grava nada no Jira. |

O fluxo da `qa-create-test-case` está desenhado em [`qa-create-test-case/fluxo.html`](qa-create-test-case/fluxo.html). Abra o arquivo no navegador.

## Marketplace do time

A `qa-create-test-case` também é distribuída como plugin pelo marketplace do time no Cursor (`devead-team/cursor-plugin-marketplace-4027107`, no origin.cursor.com). A cópia do marketplace só muda quando alguém dá push nela. Por isso o `AGENTS.md` manda publicar a skill lá a cada alteração.

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

## Figma

Se o card tiver link do Figma, a `qa-create-test-case` lê o design pelo MCP do Figma (Settings → MCP) e usa só o comportamento funcional: estados, mensagens de erro, campos obrigatórios e fluxo entre telas. A conexão é verificada no início, junto com a do Atlassian. Sem o MCP do Figma, a skill segue só com o card e registra o link como lacuna.
