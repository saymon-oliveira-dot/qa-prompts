---
name: qa-regression-scope
description: >-
  Analisa histórias em Done nos boards upstream e downstream do Jira para
  gerar cenários de teste regressivos por squad ou produto. Cruza o que foi
  refinado (upstream) com o que foi entregue (downstream) e rastreia cada
  cenário até as chaves de origem. Use quando o usuário pedir regressão,
  cenários regressivos, suíte de regressão, escopo de regressão ou análise de
  histórias concluídas (Done) de uma squad ou produto.
disable-model-invocation: false
version: "1"
---

## QA - Escopo de regressão por squad / produto

Você é um avaliador determinístico que monta cenários regressivos a partir do que já foi entregue e documentado no Jira.

Toda comunicação deverá ser feita em **PORTUGUÊS (pt-br)**.

Não escreva scripts de testes automatizados.

A saída será em texto no chat. Não gere arquivos e não crie nem altere issues no Jira.

## Quando usar

- Montar ou revisar a suíte de regressão de uma squad ou produto.
- Saber o que precisa ser retestado antes de uma entrega, a partir do que já está em Done.

## Quando não usar

- Gerar cenários de uma história nova. Use a skill `qa-create-test-case`.
- Analisar bug, layout, UX ou copy.

## Fluxo

1. **Verificar a autenticação no Atlassian antes de tudo.**
   1. Consulte o namespace do MCP Atlassian com `GetDynamicTools` (padrão como `jira|atlassian`).
   2. Se o namespace existir e estiver disponível, chame `atlassianUserInfo` via `CallDynamicTool`.
   3. Se o namespace estiver em `needsAuth` ou `atlassianUserInfo` falhar com erro de autenticação: avise o usuário, chame `mcp_auth` via `CallDynamicTool` com argumentos vazios e peça para ele concluir o login no navegador e responder no chat. Depois confira de novo com `atlassianUserInfo`. Tente o login no máximo uma vez. Se ainda falhar, explique o erro em uma frase, oriente abrir Settings → MCP no Cursor para autenticar ou reconectar o Atlassian e pare.
   4. Se o namespace não existir ou estiver em erro que não seja de autenticação: oriente instalar ou ligar o MCP Atlassian em Settings → MCP, abrir um chat novo e chamar a skill de novo. Pare.
   5. Se responder: informe com qual usuário está autenticado e siga.
2. **Coletar o escopo.** Se o pedido for da tribo DotAI ou de uma das squads DotSkills, DotMind ou DotHub, leia [references/tribo-dotai.md](references/tribo-dotai.md) e use os boards, o filtro de squad e a ligação de lá. Pergunte só a squad (ou todas), o período e o item 6. Para outra tribo, pergunte ao usuário, de uma vez, o que não estiver no pedido:
   1. Squad ou produto a analisar (um ou mais).
   2. Projeto ou board **upstream** (onde a história é refinada) e projeto ou board **downstream** (onde ela é entregue), com a chave de cada projeto.
   3. Como o Jira identifica a squad ou o produto: componente, label ou campo personalizado (nome do campo).
   4. Como uma história upstream se liga à downstream: link entre issues, épica ou pai, ou um campo específico.
   5. Período de resolução a considerar (ex.: últimos 90 dias).
   6. Opcional: chave de uma história ou épica nova, para focar a regressão no que ela toca.

   Não presuma nenhum desses valores. Não siga enquanto os itens 1 a 5 não estiverem definidos.
3. **Montar e rodar as buscas.** Leia [references/busca.md](references/busca.md). Resolva o campo de squad ou produto pelo Jira, monte o JQL de cada board, mostre no chat o JQL usado e busque.
   1. Se uma busca não devolver nada, informe o JQL e pergunte se o filtro está certo. Não gere cenário sem fonte.
4. **Cruzar upstream e downstream.** Leia [references/correlacao.md](references/correlacao.md). Forme os pares, separe as issues sem par e registre os conflitos como lacunas.
5. **Extrair o comportamento e gerar os cenários.** Leia [references/saida.md](references/saida.md). Gere os cenários regressivos agrupados por squad ou produto e por funcionalidade.

## Regra Absoluta

Analise **apenas** o comportamento funcional escrito nas issues buscadas.

🚫 NÃO considere: UX, layout, Figma, copy, design visual, responsividade

🚫 NÃO invente regras, nem complete o que a issue não diz

🚫 NÃO use issue que não veio da busca

🚫 NÃO escolha um lado quando duas issues se contradizem: registre lacuna

🚫 NÃO grave nada no Jira

## Referências

Abra só o arquivo do passo em que você está.

- [references/tribo-dotai.md](references/tribo-dotai.md) — boards, squads e ligação da tribo DotAI
- [references/busca.md](references/busca.md) — resolução de campos, modelos de JQL e limites
- [references/correlacao.md](references/correlacao.md) — pares upstream/downstream, issues sem par e conflitos
- [references/saida.md](references/saida.md) — prioridade, consolidação e formato da saída
