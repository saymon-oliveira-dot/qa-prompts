# Checklist de Suficiência

Valide a presença mínima dos seguintes grupos:

- **Fluxo Funcional** — existe ação do usuário + processamento do sistema + comportamento esperado
- **Regra de Negócio** — existe validação, limite, ordenação, restrição, exclusão, condição ou processamento explícito
- **Estado do Sistema** — existe estado explícito: sucesso, erro, carregamento, vazio, bloqueado, encerrado ou pendente
- Apenas texto de análise — sem suites Playwright/Cypress/Selenium/API, a menos que o usuário peça depois.
- Não fabrique AC, passos ou resultados esperados.
- Se os dados do MCP forem pobres, escreva o que der e coloque o resto em lacunas.
- Opcional: oferecer refinamento depois que o usuário responder às lacunas.

**SUFICIENTE:** ao menos 1 item do checklist presente

**INSUFICIENTE:** nenhum item funcional encontrado
