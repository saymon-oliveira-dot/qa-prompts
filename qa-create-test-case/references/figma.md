# Análise de Figma

Use só quando a estória, uma subtarefa ou um comentário tiver link do Figma (`figma.com/design/...`, `figma.com/file/...`, `figma.com/proto/...`). Sem link, não procure Figma nem mencione Figma na saída.

## Como ler

1. Junte todos os links do Figma encontrados na estória, nas subtarefas e nos comentários. Anote de onde veio cada um (chave da issue).
2. Se o MCP do Figma ficou indisponível na verificação inicial, não tente ler. Registre uma lacuna `baixa` por link: "Figma não analisado: MCP do Figma indisponível. <link> (<chave de origem>)".
3. Com o MCP disponível, descubra as tools com `GetDynamicTools` no namespace do Figma e leia o frame indicado no link. O `node-id` da URL aponta o frame; sem `node-id`, leia a página inteira do arquivo.
4. Se a leitura falhar (sem permissão, arquivo removido, link quebrado), registre uma lacuna `baixa` com o link e o erro devolvido, e siga sem o Figma.

## O que usar do Figma

Só comportamento funcional visível no design:

- estados de tela: vazio, carregando, erro, sucesso, desabilitado, bloqueado
- mensagens de erro ou validação e a condição em que aparecem
- campos obrigatórios, opcionais e limites indicados no design
- fluxo entre telas: qual ação leva a qual tela ou estado
- regras escritas em anotações ou comentários do arquivo

Mensagem de erro conta como comportamento (aparece uma mensagem quando X acontece). O texto exato da mensagem só entra no resultado esperado se a estória ou um critério de aceite exigir esse texto.

## O que não usar

Cores, tipografia, espaçamento, alinhamento, ícones, layout, responsividade e copy de marketing. Não gere cenário para conferir aparência.

## Como combinar com o card

- O Figma é fonte junto com a estória e as subtarefas: entra no checklist de suficiência e nas heurísticas.
- Cenário que vem do Figma rastreia o frame: `Figma / <nome do frame>` com link para o frame, além da chave da issue onde o link estava.
- Se o Figma contradizer a estória ou uma subtarefa, não escolha um lado. Registre a contradição como lacuna citando o frame e a chave da issue.
- Comportamento que só aparece no Figma e não tem nenhuma menção no card vira cenário com rastreio no Figma e uma lacuna `baixa` perguntando se ele faz parte do escopo.
