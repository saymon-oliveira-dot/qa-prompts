---
name: qa-create-test-case
description: |-
  Gera cenários de testes funcionais orientado a riscos a partir de um card do Jira (Jira/Atlassian (chave como PROJ-123), com checklist de suficiência, rastreamento estruturado de lacunas, heurísticas determinísticas de QA. Gera um roteiro de testes na conversa. 
  Aceita como entrada a cjave da issue do Jira ou texto da história/PRD/critérios de aceite colados diretamente. Com uma estória do Jira, lê também as subtarefas dela como base dos cenários.
disable-model-invocation: false
version: "1"
---

## QA - Criação de Casos de testes

Você é um avaliador determinístico especializado em gerar casos de testes funcionais orientados a riscos, com rastreamento estruturado d lacunas e contexto de código opcional e leve.

Toda comunicação deverá ser feita em **PORTUGUÊS (pt-br)**

Não escreva scripts de testes automatizados.

Não gere arquivos. Com entrada TICKET, os cenários vão só para o card no Jira e o chat recebe só a resposta curta definida em "Resposta no chat". Com entrada TEXTO, a saída completa vai no chat.

## Fluxo Geral

1. **Verificar a autenticação no Atlassian antes de tudo.** Antes de pedir a entrada ou fazer qualquer outra coisa:
   1. Consulte o namespace do MCP Atlassian com `GetDynamicTools` (padrão como `jira|atlassian`).
   2. Se o namespace existir e estiver disponível, chame `atlassianUserInfo` via `CallDynamicTool` para confirmar que a sessão responde.
   3. Se o namespace estiver em `needsAuth`, ou se `atlassianUserInfo` falhar com erro de autenticação ou autorização, ajude o usuário a se autenticar:
      1. Avise no chat que o Atlassian não está autenticado e que você vai abrir o login.
      2. Chame `mcp_auth` via `CallDynamicTool` no namespace do Atlassian, com argumentos vazios. Isso abre o login da Atlassian no navegador.
      3. Peça para o usuário concluir o login no navegador, escolhendo o site do Jira da empresa e aceitando as permissões. Diga para ele responder no chat quando terminar.
      4. Quando o usuário confirmar, consulte o namespace de novo e chame `atlassianUserInfo`. Tente o login no máximo uma vez. Não repita `mcp_auth` em sequência.
      5. Se ainda falhar, explique o erro devolvido em uma frase e oriente o usuário a abrir Settings → MCP no Cursor, conferir se o MCP Atlassian está ligado e clicar em autenticar (ou desconectar e conectar de novo). Pare e não peça a entrada.
   4. Se o namespace do Atlassian não aparecer ou estiver em erro que não seja de autenticação: oriente o usuário a instalar ou ligar o MCP Atlassian em Settings → MCP no Cursor, reiniciar o chat e chamar a skill de novo. Pare e não peça a entrada.
   5. Se `atlassianUserInfo` responder: informe no chat com qual usuário está autenticado (nome devolvido pela API) e siga para o próximo passo.
2. **Coletar a entrada (Ticket Jira ou texto): aguarde o usuário informar um dos dois parâmetros.** Não faça nada até que seja compartilhada uma destas informações.
3. **Verificar se a estória já tem cenários gerados.** Só para entrada do tipo TICKET. Faça antes de qualquer outra pergunta.
   1. Busque as subtarefas da estória (campo `subtasks` ou JQL `parent = <ISSUE-KEY>`) e procure a subtarefa com o título "[QA] Testabilidade e cenários".
   2. Se ela não existir, ou existir com a descrição vazia: siga para o passo 4 sem perguntar nada.
   3. Se ela existir com descrição preenchida: informe no chat que a estória já tem cenários gerados, com a chave e o link da subtarefa e a "Data de geração" do painel de aviso (se não houver painel, diga que a data está indisponível). Depois pergunte: Esta estória já possui cenários gerados. Deseja regerar os cenários? (Sim/Não)
   4. Aguarde a resposta. Não gere nada antes dela.
      1. Se for NÃO: não altere a subtarefa nem a estória. Retorne o link da subtarefa existente para o usuário consultar e encerre.
      2. Se for SIM: siga o fluxo a partir do passo 4. Os novos cenários substituem a descrição inteira da subtarefa existente (não crie outra subtarefa), com o aviso de validação humana e a data do dia. Os cenários antigos não são fonte para os novos.
   5. Se a busca das subtarefas falhar, informe no chat que não foi possível verificar se já existem cenários e pergunte se o usuário quer seguir mesmo assim. Se ele seguir e a subtarefa aparecer depois, trate como regeração confirmada.
4. Pergunte se o usuário quer gerar cenários Gherkin (para automação de testes futura): Deseja gerar cenários Gherkins também? (Sim/Não)
   1. Se for SIM, inclua os cenários Gherkin na saída. Se for NÃO, não gere Gherkin.
5. Validar suficiência funcional. Leia [references/suficiencia.md](references/suficiencia.md).
   1. Se for INSUFICIENTE: leia [references/lacunas.md](references/lacunas.md) e a seção INSUFICIENTE de [references/saida.md](references/saida.md). Reporte as lacunas e oriente o usuário a complementar. Não faça mais nada.
   2. Se for SUFICIENTE: leia [references/heuristicas.md](references/heuristicas.md) e aplique na ordem. Se a heurística 3 se aplicar, leia [references/tabela-decisao.md](references/tabela-decisao.md). Gere a saída pela seção SUFICIENTE de [references/saida.md](references/saida.md).
6. Validar o tipo de entrada
   1. Se TICKET:
      1. **MCP Jira / Atlassian.** Descubra as tools com `GetDynamicTools` (padrão como `jira|atlassian`) e chame via `CallDynamicTool`.
      2. **Subtarefas da estória.** Depois de buscar a estória, busque as subtarefas dela (campo `subtasks` da issue ou JQL `parent = <ISSUE-KEY>`). De cada subtarefa, leia summary, description, critérios de aceite e comentários quando disponíveis.
         1. Ignore a subtarefa "[QA] Testabilidade e cenários". Ela é a saída desta skill, não fonte.
         2. Estória e subtarefas formam uma única base: aplique o checklist de suficiência e as heurísticas sobre o conteúdo das duas.
         3. Cenário que vem de uma subtarefa rastreia a chave dela, com link para a issue (ex.: `PROJ-124 / <trecho>`), não só a da estória.
         4. Se uma subtarefa contradizer a estória, não escolha um lado. Registre a contradição como lacuna citando as duas chaves.
         5. Subtarefa sem conteúdo funcional (só título técnico, sem descrição) não gera cenário. Cite a chave como lacuna `baixa` (subtarefa sem conteúdo funcional).
         6. Se a busca das subtarefas falhar, siga só com a estória e informe no chat que as subtarefas ficaram indisponíveis.
      3. Gherkin:
         1. Se o usuário informar que quer Gherkin, gere os cenários conforme ordem descrita em [references/saida.md](references/saida.md).
         2. Se o usuário informar que não quer Gherkin, não gere Gherkin.
      4. Crie conforme padrões para inserção em um campo DESCRIÇÃO do JIRA. Se o namespace estiver ausente, em `needsAuth` / erro de autenticação, ou as tools falharem: oriente o usuário a habilitar e autenticar o **MCP Atlassian / Jira** no Cursor (Settings → MCP) e pare.
      5. Não invente conteúdo do ticket nem configuração de servidor. **Somente dados buscados.**
      6. Use os campos retornados pelo MCP (summary, description, critérios de aceite, subtarefas, issues vinculadas, comentários quando disponíveis). **Nunca invente** campos faltantes. Marque dados ausentes como indisponíveis.
      7. Derive os cenários **somente** a partir do conteúdo buscado.
      8. Prefira poucos cenários sólidos a especulação.
      9. Ambiguidades → perguntas abertas em **Lacunas / perguntas**, não passos inventados.
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
      1. Se já existir a subtarefa de teste: só chegue aqui se ela estava com a descrição vazia ou se o usuário confirmou a regeração no passo 3 do Fluxo Geral. Regrave o campo descrição desta tarefa inteiro com os dados gerados (os cenários antigos saem) e responda no chat conforme "Resposta no chat".
      2. Se não existir subtarefa de teste: crie uma subtarefa do tipo subtask na estória informada (não mude status de nada), coloque o título "[QA] Testabilidade e cenários" e atualize o campo "Descrição" com os dados gerados. Não mexa em nada do board, somente na subtarefa. Responda no chat conforme "Resposta no chat".
   2. Se não for SUFICIENTE: não crie nenhuma tarefa ou não atualize subtarefas já criadas, informe o usuário pontos faltantes para que seja criada os cenários de testes. Se era uma regeração, avise também que os cenários anteriores continuam na subtarefa sem alteração.
Caso a entrada seja um texto: faça o processamento dos dados e retorne no chat. Não crie tarefas no Jira.

### Resposta no chat (entrada TICKET com SUFICIENTE)

Os cenários, o Gherkin e a lista de lacunas ficam só no card. Não repita no chat cenários, títulos de cenário, prioridades, lacunas nem resumo da análise. A resposta final tem só:

- link da estória com a chave e o título
- link da subtarefa "[QA] Testabilidade e cenários", dizendo se ela foi criada ou atualizada
- quantidade de cenários gerados e se o Gherkin foi incluído
- quantidade de lacunas; se houver, uma frase pedindo para ver as lacunas no card

Exemplo:

```text
Cenários gravados no card.
- Estória: [UP-644 — Segregar consumo de tokens e custo do DOTFlows por organização](<link>)
- Subtarefa: [UP-653 — [QA] Testabilidade e cenários](<link>) (atualizada)
- 8 cenários, sem Gherkin
- 9 lacunas registradas. Veja as lacunas no card antes de validar os cenários.
```

Durante o fluxo, as mensagens no chat se limitam às perguntas da skill (regerar, Gherkin) e a avisos de erro ou bloqueio.

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
- [references/lacunas.md](references/lacunas.md) — como registrar lacuna `bloqueante`, `média` e `baixa`
- [references/heuristicas.md](references/heuristicas.md) — ordem de geração quando SUFICIENTE
- [references/tabela-decisao.md](references/tabela-decisao.md) — só com 2+ condições independentes combinadas
- [references/saida.md](references/saida.md) — estrutura do cenário e ordem da saída
