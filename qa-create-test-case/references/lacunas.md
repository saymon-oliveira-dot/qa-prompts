# Análise de Lacunas

Use quando o veredito for INSUFICIENTE, ou quando itens do checklist estiverem parcialmente cobertos.

Em vez de uma rejeição única e plana, registre cada informação faltante ou ambígua como uma lacuna rastreada, em lista com bullets (nunca tabela), numeradas `L1`, `L2`, … com o trecho até os dois-pontos em negrito:

```text
- **L1 (<severidade>, <item do checklist afetado>):** <descrição em forma de pergunta>
```

Toda chave de issue citada na lacuna vira link, como em [saida.md](saida.md).

- Item do checklist afetado: `Fluxo Funcional`, `Regra de Negócio` ou `Estado do Sistema`.
- Severidade, sempre em português: `bloqueante` (nenhum teste possível), `média` (limita a profundidade do teste) ou `baixa` (desejável, não essencial).

- `bloqueante` → nenhum dos itens do checklist está presente, consistente com o veredito INSUFICIENTE do Checklist de Suficiência → nenhum cenário é gerado.
- `média` / `baixa` → ao menos 1 dos 3 itens do checklist está presente (o checklist já é SUFICIENTE), mas uma lacuna pontual limita a profundidade de alguma heurística (ex.: valores de limite, análise de erro) → gere os cenários mesmo assim e liste essas lacunas depois deles, em vez de bloquear a geração.

Não use texto genérico ("descreva ação do usuário, comportamento esperado, regras/limites, estado final do sistema"). Cada orientação de melhoria endereça uma lacuna específica da lista (mesmo ID), com uma sugestão concreta do que escrever ou esclarecer.

Assim que o usuário responder com as informações pedidas, não peça para reiniciar o comando: reprocesse a mesma solicitação imediatamente, reaplicando o Checklist de Suficiência com as novas informações. Se o resultado agora for SUFICIENTE, prossiga direto para a saída SUFICIENTE em [saida.md](saida.md).
