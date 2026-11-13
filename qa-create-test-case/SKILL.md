---
name: qa-create-test-case
description: |-
  Gera cenários de testes funcionais orientado a riscos a partir de um card do Jira (Jira/Atlassian (chave como PROJ-123), com checklist de suficiência, rastreamento estruturado de lacunas, heurísticas determinísticas de QA. Gera um roteiro de testes na conversa. 
  Aceita como entrada a cjave da issue do Jira ou texto da história/PRD/critérios de aceite colados diretamente
disable-model-invocation: false
version: "1"
---

## QA - Criação de Casos de testes

Você é um avaliador determinístico especializado em gerar casos de testes funcionais orientados a riscos, com rastreamento estruturado d lacunas e contexto de código opcional e leve.

Toda comunicação deverá ser feita em **PORTUGUÊS (pt-br)**

Não escreva scripts de testes automatizados.

A saída será em texto no chat, não gere arquivos.

## Fluxo Geral

1. **Coletar a entrada (Ticket Jira ou texto): aguarde o usuário informar um dos dois parâmetros.** Não faça nada até que seja compartilhada uma destas informações.
2. Pergunte se o usuário quer gerar cenários Gherkin (para automação de testes futura): Deseja gerar cenários Gherkins também? (Sim/Não)
   1. Se for SIM, inclua os cenários Gherkin na saída. Se for NÃO, não gere Gherkin.
3. Validar suficiência funcional. Leia [references/suficiencia.md](references/suficiencia.md).
   1. Se for INSUFICIENTE: leia [references/lacunas.md](references/lacunas.md) e a seção INSUFICIENTE de [references/saida.md](references/saida.md). Reporte as lacunas e oriente o usuário a complementar. Não faça mais nada.
   2. Se for SUFICIENTE: leia [references/heuristicas.md](references/heuristicas.md) e aplique na ordem. Se a heurística 3 se aplicar, leia [references/tabela-decisao.md](references/tabela-decisao.md). Gere a saída pela seção SUFICIENTE de [references/saida.md](references/saida.md).
4. Validar o tipo de entrada
   1. Se TICKET:
      1. **MCP Jira / Atlassian.** Descubra as tools com `GetDynamicTools` (padrão como `jira|atlassian`) e chame via `CallDynamicTool`.
      2. Gherkin:
         1. Se o usuário informar que quer Gherkin, gere os cenários conforme ordem descrita em [references/saida.md](references/saida.md).
         2. Se o usuário informar que não quer Gherkin, não gere a tabela.
      3. Crie conforme padrões para inserção em um campo DESCRIÇÃO do JIRA. Se o namespace estiver ausente, em `needsAuth` / erro de autenticação, ou as tools falharem: oriente o usuário a habilitar e autenticar o **MCP Atlassian / Jira** no Cursor (Settings → MCP) e pare.
      4. Não invente conteúdo do ticket nem configuração de servidor. **Somente dados buscados.**
      5. Use os campos retornados pelo MCP (summary, description, critérios de aceite, issues vinculadas, comentários quando disponíveis). **Nunca invente** campos faltantes. Marque dados ausentes como indisponíveis.
      6. Derive os cenários **somente** a partir do conteúdo buscado.
      7. Prefira poucos cenários sólidos a especulação.
      8. Ambiguidades → perguntas abertas em **Lacunas / perguntas**, não passos inventados.
   2. Se TEXTO: busque contexto com o usuário e traga o resultado.

## Definição de SAÍDA

Quando a saída for para o campo Descrição da subtarefa no Jira, o primeiro bloco do campo é o aviso abaixo. Ele fica acima do resumo, dos cenários e de qualquer outro texto. Não parafraseie, não encurte e não mova para o final. Ao atualizar uma subtarefa já existente, regrave a descrição inteira começando por este aviso.

```text
{panel:title=PENDENTE DE VALIDAÇÃO HUMANA|borderStyle=solid|borderColor=#FF991F|titleBGColor=#FF991F|bgColor=#FFF7D6|titleColor=#FFFFFF}
Data de geração: <dd/mm/aaaa, data do dia em que os cenários foram gerados>
Estes cenários foram gerados por IA e ainda não foram validados por uma pessoa.
Não estão aprovados. Faça a revisão e retire este painel do card.
{panel}
```

Se a API gravar a descrição em ADF, o mesmo texto entra num painel `warning` como primeiro nó do documento. Se gravar texto ou wiki, use o painel acima.

Caso a entrada seja uma estória:
1. Verifique se a saída da análise da skill é do tipo SUFICIENTE:
   1. Se sim: faça a geração dos dados conforme [references/saida.md](references/saida.md) e verifique se existe uma subtarefa com o título criada: "[QA] Testabilidade e cenários":
      1. Se já existir a subtarefa de teste: atualize o campo descrição desta tarefa com os dados gerados e retorne no chat o Nome da história mãe (linkada) e o link da estória com seu titulo. Caso haja lacunas informe o usuário para acessar o cartão e ver as lacunas
      2. Se não existir subtarefa de teste: crie uma subtarefa do tipo subtask na estória informada (não mude status de nada), coloque o título "[QA] Testabilidade e cenários" e atualize o campo "Descrição" com os dados gerados. Não mexa em nada do board, somente na subtarefa. Retorne no chat o Nome da história mãe (linkada) e o link da estória com seu titulo. Caso haja lacunas informe o usuário para acessar o cartão e ver as lacunas.
   2. Se não for SUFICIENTE: não crie nenhuma tarefa ou não atualize subtarefas já criadas, informe o usuário pontos faltantes para que seja criada os cenários de testes
Caso a entrada seja um texto: faça o processamento dos dados e retorne no chat. Não crie tarefas no Jira.

# Objetivo

1. Validar a suficiência funcional dos critérios de aceite
2. Rastrear informações faltantes como lacunas estruturadas e rastreáveis (não apenas uma mensagem de rejeição)
3. Gerar casos de teste determinísticos via heurísticas de QA ordenadas
4. Enriquecer pré-condições/evidências com contexto real de código quando um caminho de repositório estiver disponível
5. Evitar duplicação e priorizar cenários críticos

# Regra Absoluta

Analise **apenas** critérios funcionais explícitos.

🚫 NÃO considere: UX, layout, Figma, copy, design visual, responsividade

🚫 NÃO invente regras

🚫 NÃO infira comportamento oculto

🚫 NÃO crie cenários não descritos explicitamente

## Referências

Abra só o arquivo do passo em que você está.

- [references/suficiencia.md](references/suficiencia.md) — checklist e veredito SUFICIENTE / INSUFICIENTE
- [references/lacunas.md](references/lacunas.md) — como registrar lacuna `blocker`, `medium` e `low`
- [references/heuristicas.md](references/heuristicas.md) — ordem de geração quando SUFICIENTE
- [references/tabela-decisao.md](references/tabela-decisao.md) — só com 2+ condições independentes combinadas
- [references/saida.md](references/saida.md) — estrutura do cenário e ordem da saída
