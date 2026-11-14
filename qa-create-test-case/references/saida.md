# Saída

Não gere arquivos. Com entrada TICKET, esta saída vai para a descrição da subtarefa no Jira e o chat recebe só a resposta curta da seção "Resposta no chat" do `SKILL.md`. Com entrada TEXTO, esta saída vai no chat.

## Links para issues

Toda chave de issue citada na saída (estória, subtarefa, épico, issue vinculada) vira link para ela, no Rastreio, nas lacunas e no resumo. Use o site da estória buscada: `https://<site>.atlassian.net/browse/<ISSUE-KEY>`.

- HTML: `<a href="https://<site>.atlassian.net/browse/UP-431">UP-431</a>` (link comum com a chave como texto, não smart card)
- Markdown: `[UP-431](https://<site>.atlassian.net/browse/UP-431)`
- Wiki: `[UP-431|https://<site>.atlassian.net/browse/UP-431]`

Com entrada TEXTO sem site conhecido, deixe a chave sem link.

## Estrutura padrão do cenário

Todo cenário começa com uma checkbox desmarcada antes do título, para o QA marcar quando executar. O ID e o título do cenário ficam sempre em negrito, em qualquer formato de saída (Jira ou chat). A prioridade `P0`/`P1`/`P2` das heurísticas não aparece no card: ela só orienta a geração. Depois de cada cenário, deixe uma linha em branco antes do próximo. Para cada cenário:

```text
- [ ] **CT-NNN — <título>**
  - Rastreio: <ISSUE-KEY com link> / <critério de aceite ou trecho>
  - Pré-condições: ...
  - Resultado esperado: ...

```

No Jira em HTML, a checkbox é um item de task list: `<ul data-type="task-list"><li data-type="task-item"><strong>CT-NNN — <título></strong></li></ul>`, seguido de uma lista comum (`<ul>`) com Rastreio, Pré-condições e Resultado esperado, e de um parágrafo vazio (`<p></p>`) como quebra de linha antes do próximo cenário. Não marque nenhuma checkbox.

Numere os cenários `CT-001`, `CT-002`, … Títulos curtos e com intenção clara.

## Formato das lacunas

Lacunas são uma lista com bullets, nunca tabela. Uma lacuna por bullet, numeradas `L1`, `L2`, … O trecho do início até os dois-pontos fica em negrito:

```text
- **L1 (<severidade>, <item do checklist afetado>):** <pergunta analítica>
```

Em HTML: `<li><p><strong>L1 (média, Regra de Negócio):</strong> pergunta</p></li>`.

Registre as lacunas como em [lacunas.md](lacunas.md).

## Ordem da saída INSUFICIENTE

🚫 Não gere cenários quando INSUFICIENTE.

```text
INSUFICIENTE: Critérios funcionais insuficientes para geração de testes.
Descrição de lacunas em forma de perguntas para alinhamento com devs.

---
LACUNAS:
- **L1 (bloqueante, Fluxo Funcional):** ...
```

🚫 Não use texto genérico ("descreva ação do usuário, comportamento esperado, regras/limites, estado final do sistema") — cada linha de COMO MELHORAR deve endereçar uma lacuna específica da lista LACUNAS (mesmo ID), com uma sugestão concreta do que escrever/esclarecer.

Assim que o usuário responder com as informações pedidas, não peça para reiniciar o comando: reprocesse a mesma solicitação imediatamente, reaplicando o checklist em [suficiencia.md](suficiencia.md). Se o resultado agora for SUFICIENTE, prossiga para a ordem abaixo.

## Ordem da saída SUFICIENTE

Quando o destino for a descrição da subtarefa no Jira, o primeiro bloco é o aviso de validação humana definido em `SKILL.md`. Ele vem antes dos itens abaixo.

1. **Resumo do card** (curto, até 300 caracteres), seguido de uma linha horizontal (`<hr>` em HTML, `----` em wiki, `---` em markdown). Não liste fontes nem subtarefas usadas no resumo.
2. **Cenários**, em três grupos e nesta ordem. Cada grupo é um título seguido dos cenários daquele grupo (estrutura padrão deste arquivo, com checkbox e linha em branco entre cenários). A numeração `CT-001`, `CT-002`, … continua de um grupo para o outro. Omita o título do grupo que não tiver cenário.
   - **Cenários felizes** — caminho feliz, cenário positivo de regra de negócio e estado de sucesso.
   - **Cenários de exceção e autorização** — cenário negativo de regra, teste negativo, erro explícito e bloqueio por permissão, autorização ou origem.
   - **Cenários de limite e transição** — valores de limite e transição de estado (entrada, progressão, conclusão e bloqueio por limite, timeout, quota ou retentativa).
3. **Cenários Gherkin** (quando o usuário pedir)
4. **Lacunas / perguntas**, precedidas de uma linha horizontal antes do título — AC faltando, regras pouco claras, afirmações não testáveis, necessidades de dados/ambiente. Sempre em forma de pergunta analítica, no formato de lista deste arquivo.

🚫 Não inclua linha de fonte, "Campo ambiente de testes", "Campo massa de teste" nem a frase "Geração Automatizada de Casos de Teste via IA".

Use o exemplo abaixo como base:

```text
Resumo do que se trata o card em até 300 caracteres
---

Cenários felizes
- [ ] **CT-001 — <título>**
  - Rastreio: [UP-431](<link>) / ...
  - Pré-condições: ...
  - Resultado esperado: ...

Cenários de exceção e autorização
- [ ] **CT-002 — <título>**
  - Rastreio: ...
  - Pré-condições: ...
  - Resultado esperado: ...

Cenários de limite e transição
- [ ] **CT-003 — <título>**
  - Rastreio: ...
  - Pré-condições: ...
  - Resultado esperado: ...

Cenários Gherkin (quando informada a necessidade)

---
Lacunas/Pontos de atenção (quando identificada a necessidade)
- **L1 (média, Regra de Negócio):** ... na [UP-431](<link>) ...
```
