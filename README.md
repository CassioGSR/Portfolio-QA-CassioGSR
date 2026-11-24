# Portfólio QA: Cassio

[![Cypress Tests](https://github.com/CassioGSR/Portifolio-QA-CassioGSR/actions/workflows/cypress.yml/badge.svg)](https://github.com/CassioGSR/Portifolio-QA-CassioGSR/actions/workflows/cypress.yml)

Automação de testes end-to-end do sistema OrangeHRM (versão demo pública), com Cypress e JavaScript. O projeto inclui plano de testes, casos de teste rastreáveis até o código e execução automática em CI.

**Em números:** 17 testes automatizados · 5 áreas do sistema · relatório HTML a cada execução · pipeline no GitHub Actions

**Navegação rápida:**

- [Automação](automacao-cypress/)
- [Plano de testes](documentacao/plano-de-testes.md)
- [Casos de teste](documentacao/casos-de-teste.md)

## Sobre o projeto

O objetivo é validar, de ponta a ponta, os fluxos principais do OrangeHRM: login, dados pessoais, navegação entre módulos, busca de usuários e cadastro de funcionários. Com ele, busco demonstrar o conhecimento prático acerca de automação de testes com Cypress e JavaScript, testes documentados e execução automática em CI, organização de código com Page Object Model. O projeto foi construído em etapas, e as decisões e limitações ficam registradas na documentação.

- **Sistema testado:** [OrangeHRM (demo público)](https://opensource-demo.orangehrmlive.com)
- **Usuário de teste:** `Admin` / `admin123` (credenciais públicas do demo)

## Tecnologias

| Tecnologia | Para que foi usada |
|---|---|
| Cypress 16 | Automação dos testes ponta a ponta |
| JavaScript | Linguagem dos testes |
| Page Object Model | Seletores e ações organizados por tela |
| Chance | Geração de dados de teste aleatórios |
| cypress-mochawesome-reporter | Relatório HTML de cada execução |
| GitHub Actions | Execução automática dos testes (CI) |
| Prettier | Padronização do estilo do código |
| Git e GitHub | Versionamento e hospedagem |

## Cenários cobertos

São 17 casos de teste, todos documentados com passos e resultado esperado em [casos-de-teste.md](documentacao/casos-de-teste.md).

| Área | Casos | O que é validado |
|---|---|---|
| Login | CT-001 a CT-005 | Login válido, credenciais inválidas, campos vazios e logout |
| My Info | CT-006 a CT-008 | Atualização de dados, campo obrigatório e persistência após recarregar |
| Navegação | CT-009 a CT-013 | Acesso aos módulos Admin, PIM, Leave, Time e Performance |
| Admin | CT-014 e CT-015 | Busca de usuário existente e inexistente |
| PIM | CT-016 e CT-017 | Cadastro e busca de funcionário |

O ID de cada caso aparece no título do teste (por exemplo, `CT-003: login - fail with wrong password`), o que liga a documentação, o código e o relatório.

## Estrutura do projeto

```
portifolio-qa-cassio/
├── .github/workflows/cypress.yml   # pipeline de CI
├── automacao-cypress/
│   ├── cypress/
│   │   ├── e2e/                    # testes: login, myInfo, navigation, admin, pim
│   │   ├── fixtures/               # dados de teste (usuários)
│   │   ├── pages/                  # Page Objects (um por tela)
│   │   └── support/                # comando cy.login() e configurações globais
│   ├── cypress.config.js
│   └── package.json
├── documentacao/                   # plano de testes e casos de teste
├── evidencias/                     # prints do relatório e do CI
├── .prettierrc
└── README.md
```

## Como executar

**Pré-requisitos:** Node.js 22, 24 ou 26 ou superior (requisito do Cypress 16; o projeto foi desenvolvido com Node 24) e Git.

```bash
git clone https://github.com/CassioGSR/Portifolio-QA-CassioGSR.git
cd Portifolio-QA-CassioGSR/automacao-cypress
npm ci
```

| Comando | O que faz |
|---|---|
| `npm test` | Roda todos os testes no terminal |
| `npx cypress run --browser chrome` | Mesmo, no Chrome (é o que o CI usa) |
| `npm run cy:open` | Abre a interface interativa do Cypress |
| `npm run test:video` | Roda gravando vídeos em `cypress/videos/` |

O relatório HTML da última execução fica em `automacao-cypress/cypress/reports/html/index.html`.

## Relatórios e integração contínua

A cada push ou Pull Request na branch `main`, o GitHub Actions executa a suíte completa no Chrome e publica o relatório HTML como artefato da execução (guardado por 14 dias). Se algum teste falhar, os screenshots também são publicados. A execução também pode ser disparada manualmente pela aba **Actions**.

![Relatório de execução](evidencias/relatorio-mochawesome.png)

## Decisões técnicas

| Decisão | Motivo |
|---|---|
| Page Object Model | Seletores e ações de cada tela ficam em um único arquivo, e uma mudança no site é corrigida em um só lugar |
| Seletores por texto e label | Evitam quebrar quando a ordem dos elementos na tela muda |
| `cy.login()` com `cy.session()` | O login é feito uma vez e reaproveitado, deixando a suíte mais rápida |
| `cy.intercept()` com `cy.wait('@alias')` | O teste espera a resposta do servidor em vez de usar pausas de tempo fixo |
| Dados aleatórios (Chance) | Cada execução gera dados novos, e os testes criam o que precisam, sem depender de dados prévios |
| Tratamento restrito de erros JavaScript do site | Dois erros internos conhecidos do OrangeHRM são ignorados, e qualquer outro erro continua reprovando o teste |

## Limitações conhecidas

- **Ambiente público e compartilhado:** o demo do OrangeHRM é reiniciado periodicamente e às vezes fica lento ou responde com erro de servidor. Já houve uma execução vermelha por isso, e a reexecução resolveu. Os testes não usam repetição automática (retries): uma falha é investigada primeiro e, se for do ambiente, o job é reexecutado. A decisão está registrada no plano de testes.
- **Dados criados e não removidos:** os testes de PIM cadastram funcionários e não os apagam. No demo isso não importa. Em outro ambiente, a limpeza seria feita via API.
- **Alertas de dependências aceitos:** três alertas moderados do `npm audit` na cadeia do gerador de relatório não têm correção compatível e estão documentados como risco aceito.
- **Fora do escopo:** testes de API, desempenho, acessibilidade, vários navegadores e dispositivos móveis.

## Possíveis evoluções

- Testes de API com `cy.request`
- Limpeza dos dados criados via API ao final dos testes de PIM
- Atualização automática de dependências com Dependabot
- Relatórios de bug em `relatorios-de-bug/`

## Contato

Cassio Gabriel Silva Rodrigues · QA em formação · PARAUAPEBAS/PARÁ

- LinkedIn: www.linkedin.com/in/cassio-rodrigues-2014973a5
- E-mail: cassiogsrodrigues@gmail.com