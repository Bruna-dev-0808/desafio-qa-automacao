# Projeto de Automação QA — Desafio Técnico

Automação de testes de **API** (Postman/Newman) e **UI/E2E** (Cypress + BDD Cucumber/Gherkin) para o e-commerce https://www.demoblaze.com.

**Autora:** Bruna Karoline Ferraz

## 🚀 Tecnologias Utilizadas
- Cypress 13 + JavaScript
- BDD com Cucumber/Gherkin (`@badeball/cypress-cucumber-preprocessor`), cenários escritos em português
- Padrão Page Objects
- Postman + Newman (execução da collection via linha de comando)
- Relatórios: Cucumber HTML Report e Newman HtmlExtra
- GitHub Actions (CI)

## 📋 Pré-requisitos
- Node.js 20 LTS (18+ também funciona) — versão indicada em `.nvmrc`
- Git
- Acesso à internet (os testes rodam contra o site demo e a API pública do Demoblaze)

## 🔧 Configuração e Instalação
```bash
# Como clonar o projeto
git clone https://github.com/seu-usuario/desafio-qa-automacao.git
cd desafio-qa-automacao

# Como instalar as dependências
npm install
```

## 🧪 Como Executar os Testes

### Testes de API (Postman / Newman)
```bash
npm run test:api
# ou, direto:
npx newman run postman/demoblaze_api_collection.json -e postman/demoblaze_environment.json
```
Também é possível importar os dois arquivos da pasta `postman/` no app do Postman (Collection + Environment).

### Testes Web (UI / BDD)
```bash
npm run test          # headless (Electron)
npx cypress open      # modo interativo
npm run test:ui:conta     # só o cenário de criação de conta
npm run test:ui:carrinho  # só o cenário de carrinho
```

### Tudo de uma vez
```bash
npm run test:all
```

## 📊 Relatórios de Testes
- API: `reports/newman/api-report.html`
- UI (Cucumber HTML): `reports/cucumber/cucumber-report.html` (e `.json`)
- Screenshots de falha: `reports/screenshots/`

## 🗂️ Estrutura
```
├── postman/
│   ├── demoblaze_api_collection.json     # Collection (conta + catálogo + carrinho)
│   └── demoblaze_environment.json        # Variáveis de ambiente
├── cypress/
│   ├── e2e/                              # Feature files (Gherkin)
│   │   ├── criacao_conta.feature
│   │   └── carrinho.feature
│   ├── fixtures/produtos.json            # Massa de testes (produtos/categorias)
│   └── support/
│       ├── pages/                        # Page Objects
│       ├── step_definitions/             # Steps do BDD
│       ├── data/userFactory.js           # Massa dinâmica (usuário novo a cada execução)
│       ├── helpers/alert.js              # Captura/validação de window.alert
│       ├── commands.js                   # Comandos customizados (cadastro via API)
│       └── e2e.js
├── .github/workflows/ci.yml              # Pipeline GitHub Actions
├── cypress.config.js
└── package.json
```

## ✅ Cobertura

**API (Postman)** — cadastro (sucesso e usuário existente), login (sucesso, senha incorreta, usuário inexistente), listagem de produtos por categoria, adicionar 3 itens ao carrinho, visualizar, remover o monitor e validar que restam 2 itens. Asserções de status code, JSON Schema (contrato), tempo de resposta (`maxResponseTime`) e mensagens de erro/sucesso. Usa variáveis de ambiente (`baseUrl`, `username`, `authToken`, ids dos produtos etc.).

**UI (BDD)**
- Cenário 1 — Criação de conta: cadastro válido, tentativa com e-mail já existente e tentativa com campos vazios.
- Cenário 2 — Carrinho: adiciona celular, monitor e computador; acessa o carrinho; remove o monitor; atualiza a página; confirma que restam apenas celular e computador.

## 📝 Observações
- O Demoblaze usa o campo *username* para cadastro; os testes usam um valor em formato de e-mail (`qa.<timestamp>@teste.com`), gerado dinamicamente para não colidir entre execuções.
- A API do Demoblaze retorna **HTTP 200 também nos erros de negócio** (ex.: usuário já existente); por isso os cenários negativos validam o campo `errorMessage` do corpo, e não o status code.
- O site informa sucesso/erro por `window.alert`, que é interceptado pelo Cypress (`helpers/alert.js`).
- Diferenciais incluídos: relatórios automáticos, CI com GitHub Actions, execução headless, testes negativos.
