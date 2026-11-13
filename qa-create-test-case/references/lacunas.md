# Análise de Lacunas

Use quando o veredito for INSUFICIENTE, ou quando itens do checklist estiverem parcialmente cobertos.

Em vez de uma rejeição única e plana, registre cada informação faltante ou ambígua como uma lacuna rastreada:

| ID | Descrição | Item do Checklist Afetado | Severidade |
|---|---|---|---|
| G1 | ... | Fluxo Funcional / Regra de Negócio / Estado do Sistema | `blocker` (nenhum teste possível) / `medium` (limita a profundidade do teste) / `low` (desejável, não essencial) |

- `blocker` → nenhum dos itens do checklist está presente, consistente com o veredito INSUFICIENTE do Checklist de Suficiência → nenhuma tabela é gerada.
- `medium` / `low` → ao menos 1 dos 3 itens do checklist está presente (o checklist já é SUFICIENTE), mas uma lacuna pontual limita a profundidade de alguma heurística (ex.: valores de limite, análise de erro) → gere a tabela mesmo assim, e liste essas lacunas após ela como "Limitações Conhecidas" em vez de bloquear a geração.

Não use texto genérico ("descreva ação do usuário, comportamento esperado, regras/limites, estado final do sistema"). Cada orientação de melhoria endereça uma lacuna específica da tabela (mesmo ID), com uma sugestão concreta do que escrever ou esclarecer.

Assim que o usuário responder com as informações pedidas, não peça para reiniciar o comando: reprocesse a mesma solicitação imediatamente, reaplicando o Checklist de Suficiência com as novas informações. Se o resultado agora for SUFICIENTE, prossiga direto para a saída SUFICIENTE em [saida.md](saida.md).
