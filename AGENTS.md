# AGENTS.md

Guia para agentes que criam e mantêm as skills de QA da DotAI. Leia este arquivo antes de adicionar ou alterar qualquer skill.

## Propósito

Este repositório é a fonte das skills de QA usadas no Cursor. Cada skill ensina um fluxo de qualidade: o que fazer, em que ordem, o que é proibido e como a saída deve parecer.

Uma skill, um fluxo. Não misture geração de casos de teste, revisão de bug e automação no mesmo `SKILL.md`.

## Idioma

Escreva skills, exemplos e a comunicação com o usuário em português (pt-BR).

## Onde as skills ficam

```text
.cursor/skills/<nome-da-skill>/
├── SKILL.md          # obrigatório
├── references/       # opcional; detalhe que o agente lê só quando precisa
├── examples.md       # opcional
└── scripts/          # opcional; só se o fluxo exigir um script fixo
```

O nome da pasta é o `name` do frontmatter: minúsculas, números e hífens, no máximo 64 caracteres, com prefixo `qa-`. Exemplo: `qa-create-test-case`.

## Como escrever uma skill

1. Leia o `SKILL.md` inteiro antes de editar. Preserve o que já está correto.
2. Se o usuário ditar um trecho exato, copie esse trecho palavra por palavra.
3. Coloque no `SKILL.md` só o que o agente precisa para executar o fluxo. Detalhe longo vai para `references/`, com link direto a partir do `SKILL.md` (um nível apenas).
4. Mantenha o `SKILL.md` abaixo de 500 linhas.
5. A `description` é o gatilho de descoberta. Escreva em terceira pessoa, com o que a skill faz e quando usá-la (termos como Jira, caso de teste, critério de aceite, bug, regressão).

Frontmatter mínimo:

```yaml
---
name: qa-nome-do-fluxo
description: >-
  O que a skill faz e quando o agente deve usá-la.
  Inclua os termos que o usuário realmente diz.
disable-model-invocation: false
---
```

Use `disable-model-invocation: false` quando a skill deve entrar sozinha a partir do pedido do usuário. Use `true` somente quando ela deve carregar apenas se for chamada pelo nome.

Corpo esperado:

- quando usar e quando não usar
- fluxo em passos, com a decisão que interrompe o trabalho
- regras absolutas (o que não inventar)
- formato de saída, com um exemplo curto
- lacunas: o que falta e como o usuário completa

## Invariantes de QA

Valem para toda skill, salvo se o próprio `SKILL.md` abrir uma exceção explícita.

- Trabalhe só com o que está escrito na fonte (card, critério de aceite, texto colado, código lido). Não invente regra, passo, resultado esperado nem campo de ticket.
- Ambiguidade vira lacuna rastreável (o que falta, o que isso impede, como resolver). Não vire suposição.
- Prefira poucos cenários sustentados pela fonte a uma lista especulativa.
- Todo artefato de teste aponta para a origem: chave da issue, critério de aceite ou trecho.
- Fora de escopo por padrão: UX, layout, Figma, copy, visual e responsividade. Só entre se a skill for explicitamente sobre isso.
- Não escreva script de automação (Playwright, Cypress, Selenium, teste de API) a menos que a skill ou o usuário peça.
- Não fabrique dados de Jira. Busque pelo MCP Atlassian. Se o namespace estiver ausente, sem autenticação ou com erro, pare e peça para habilitar o MCP. Não preencha o que a API não devolveu: marque como indisponível.

## Ao alterar este repositório

- Mudança de comportamento de uma skill fica no `SKILL.md` dessa skill, não espalhada em vários arquivos.
- Não crie skill em `~/.cursor/skills-cursor/`. Esse diretório é do Cursor.
- Não commite segredo, token ou arquivo `.env`.
- Faça commit e pull request só quando o usuário pedir.
