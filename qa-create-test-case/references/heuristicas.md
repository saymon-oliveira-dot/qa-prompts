# Heurísticas

Aplicar nesta ordem, somente depois do veredito SUFICIENTE.

## 1. Caminho Feliz Mínimo `P0`

Ao menos 1 cenário ponta a ponta para cada ação funcional explícita.

## 2. Regra de Negócio Explícita

Para cada regra: 1 cenário positivo + 1 negativo (quando existir violação explícita).
`P0` → regra crítica | `P1` → validações secundárias

## 3. Tabela de Decisão (Condições Combinadas) `P0`/`P1`

Aplicar SOMENTE quando o texto descrever 2+ condições independentes que se combinam para determinar um resultado (ex.: tipo de cliente + faixa de valor definem o desconto aplicado). 🚫 Não aplicar para uma única condição isolada — isso já é coberto pela heurística "Regra de Negócio Explícita".

Quando esta heurística se aplicar, leia [tabela-decisao.md](tabela-decisao.md) e monte a combinação de condições × ação/resultado como passo de raciocínio (Parte 3, condicional). Gere cenários cobrindo cada combinação relevante mencionada ou logicamente decorrente do texto, nunca combinações hipotéticas não sustentadas por ele.

## 4. Estados Explícitos do Sistema

Cobrir apenas os estados explicitamente mencionados. 🚫 Não invente estados.

## 5. Valores de Limite e Representativos

Aplicar SOMENTE quando houver um número, intervalo, quantidade, timeout, máximo/mínimo.
Cobrir: limite inferior, limite superior, valor representativo válido, valor representativo inválido.
`P0` → limite crítico | `P1` → limite secundário

## 6. Transição de Estado `P0`

Quando houver ciclo, status, tentativas (retries) ou progressão: validar entrada, progressão, conclusão e bloqueio.

## 7. Teste Negativo `P1`

Gerar apenas quando explicitamente suportado pelo texto.

## 8. Análise de Erro `P1`

Aplicar apenas quando existir falha, indisponibilidade ou erro explícito. 🚫 Não invente falhas técnicas.

## 9. Priorização

- `P0` — fluxo principal, regra crítica, transição crítica, limite crítico
- `P1` — validações, negativos, exceções, falhas explícitas
- `P2` — cenários complementares explícitos

## 10. Anti-duplicação

🚫 Não gere cenários com a mesma pré-condição + ação + resultado. Consolide os semelhantes.

## 11. Rastreabilidade

Todo caso de teste deve mapear para um item do checklist, uma heurística aplicada e (se relevante) o ID de uma lacuna que ele contorna.
