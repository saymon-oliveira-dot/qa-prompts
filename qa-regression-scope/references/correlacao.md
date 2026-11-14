# Correlação upstream × downstream

## Formar os pares

Use só a forma de ligação que o usuário informou no passo de escopo:

- **Link entre issues:** a issue downstream aparece em `issuelinks` da upstream, ou o contrário.
- **Épica ou pai:** as duas compartilham a mesma épica ou o mesmo `parent`.
- **Campo específico:** o valor do campo é igual nas duas.

Uma issue upstream pode ter várias downstream. Agrupe todas no mesmo par.

## Issues sem par

- **Upstream sem downstream:** a regra foi refinada, mas a busca não mostra entrega. Liste a chave e não gere cenário regressivo a partir dela.
- **Downstream sem upstream:** a entrega existe sem o refinamento ligado. Gere cenário só se a própria issue downstream descrever o comportamento funcional. Marque o cenário com "sem upstream". Defeitos costumam nascer direto no downstream, então isso é esperado para eles.

Se a configuração da tribo permitir par por título idêntico, use esse critério só quando não houver link, e marque o par como "par por título".

## Conflitos

Registre lacuna, sem escolher um lado, quando:

- a upstream e a downstream de um mesmo par descrevem regras diferentes;
- duas issues em Done descrevem a mesma funcionalidade com resultados diferentes. A mais recente pode ter substituído a antiga, mas isso só vale se a issue disser. Se não disser, é lacuna.

Cada lacuna cita as chaves envolvidas.

## Issues sem conteúdo funcional

Issue que tem só título técnico, sem descrição de comportamento (ex.: "ajuste de pipeline", "refatoração"), não gera cenário. Liste a chave como sem conteúdo funcional.
