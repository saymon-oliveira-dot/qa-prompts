# Saída

A saída é texto no chat. Não gere arquivos.

## Estrutura padrão do cenário

Para cada cenário:

```text
### TC-NNN — <título>
- Rastreio: <ISSUE-KEY> / <critério de aceite ou trecho>
- Pré-condições: ...
- Resultado esperado: ...
```

Numere os cenários `TC-001`, `TC-002`, … Títulos curtos e com intenção clara.

## Ordem da saída INSUFICIENTE

🚫 Não gere tabela quando INSUFICIENTE.

```text
⚠️ Geração Automatizada de Casos de Teste via IA.

INSUFICIENTE: Critérios funcionais insuficientes para geração de testes.
Descrição de lacunas em forma de perguntas para alinhamento com devs.

LACUNAS:

| ID | Descrição | Item do Checklist Afetado | Severidade |
|---|---|---|---|
| G1 | ... | ... | blocker |
```

Registre as lacunas como em [lacunas.md](lacunas.md).

🚫 Não use texto genérico ("descreva ação do usuário, comportamento esperado, regras/limites, estado final do sistema") — cada linha de COMO MELHORAR deve endereçar uma lacuna específica da tabela LACUNAS (mesmo ID), com uma sugestão concreta do que escrever/esclarecer.

Assim que o usuário responder com as informações pedidas, não peça para reiniciar o comando: reprocesse a mesma solicitação imediatamente, reaplicando o checklist em [suficiencia.md](suficiencia.md). Se o resultado agora for SUFICIENTE, prossiga para a ordem abaixo.

## Ordem da saída SUFICIENTE

Quando o destino for a descrição da subtarefa no Jira, o primeiro bloco é o aviso de validação humana definido em `SKILL.md`. Ele vem antes dos itens abaixo.

1. **Resumo do card** (curto)
2. **Cenários de teste** (estrutura padrão do cenário neste arquivo)
3. **Cenários Gherkin** (quando o usuário pedir)
4. **Lacunas / perguntas** — AC faltando, regras pouco claras, afirmações não testáveis, necessidades de dados/ambiente. Sempre em forma de pergunta analítica

Use o exemplo abaixo como base:

```text
⚠️ Geração Automatizada de Casos de Teste via IA (data).

Resumo do que se trata o card em até 300 caracteres

Campo ambiente de testes: (por enquanto campo vazio)
Campo massa de teste: (por enquanto campo vazio)

Cenários de teste (estrutura padrão do cenário neste arquivo)

Cenários Gherkin (quando informada a necessidade)

Lacunas/Pontos de atenção (quando identificada a necessidade)
| ID | Descrição | Item do Checklist Afetado | Severidade |
|---|---|---|---|
| G1 | ... | ... | blocker |
```
