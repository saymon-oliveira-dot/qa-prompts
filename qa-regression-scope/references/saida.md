# Saída

A saída é texto no chat. Não gere arquivos e não grave no Jira.

## O que vira cenário regressivo

Para cada par (ou downstream sem upstream com conteúdo funcional), gere cenários só do que está escrito:

- **Fluxo principal** entregue: 1 cenário ponta a ponta.
- **Regra de negócio** explícita: 1 cenário positivo e, se a issue descrever a violação, 1 negativo.
- **Limite** explícito (número, intervalo, máximo, mínimo, timeout): limite inferior e superior.
- **Estado** explícito (sucesso, erro, bloqueado, pendente…): só os estados citados.
- **Defeito corrigido** (tipo Defeito ou Alerta em Done): 1 cenário que verifica o resultado esperado escrito no defeito, para garantir que ele não volta. Se o defeito não descrever o resultado esperado, não gere cenário e registre lacuna.

## Prioridade

- `P0` — fluxo principal e regra crítica de cada funcionalidade.
- `P1` — validações, negativos e limites.
- `P2` — demais comportamentos explícitos.

## Consolidação

- A mesma regra descrita na upstream e na downstream vira **um** cenário, com as duas chaves no rastreio.
- A mesma pré-condição + ação + resultado em issues diferentes vira um cenário só, com todas as chaves.

## Estrutura do cenário

```text
### RG-NNN — <título>
- Squad / produto: <valor>
- Funcionalidade: <nome curto, tirado das issues>
- Prioridade: P0 | P1 | P2
- Rastreio: <UPSTREAM-KEY> → <DOWNSTREAM-KEY> / <trecho ou critério>
- Pré-condições: ...
- Resultado esperado: ...
```

Numere `RG-001`, `RG-002`, … em sequência única na saída toda.

## Ordem da saída

```text
⚠️ Cenários regressivos gerados por IA (dd/mm/aaaa). Pendentes de validação humana.

Escopo
- Squad / produto: ...
- Upstream: <projeto> — JQL: ...
- Downstream: <projeto> — JQL: ...
- Período: ...
- Issues analisadas: <n> upstream, <n> downstream

Squad / produto: <valor>
  Funcionalidade: <nome>
    RG-001 ...
    RG-002 ...

Issues sem par
- Upstream sem entrega: <chaves>
- Downstream sem upstream: <chaves>

Sem conteúdo funcional: <chaves>

Lacunas
| ID | Descrição | Chaves | Impacto |
|---|---|---|---|
| G1 | ... | PROJ-1, DEV-7 | cenário não gerado / resultado esperado incerto |
```

Omita uma seção quando ela estiver vazia, exceto Escopo.
