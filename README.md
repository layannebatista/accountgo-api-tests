# AccountGo API Tests

Suite de testes automatizados para a API do [AccountGo ERP](https://github.com/accountgo/accountgo) — sistema open source desenvolvido em ASP.NET Core com frontend em Angular.

O projeto cobre os módulos **Financeiro**, **Vendas**, **Inventário** e **Comum**, com foco em validação funcional, cenários negativos, fluxos encadeados e testes de performance.

---

## Tecnologias utilizadas

- [Postman](https://www.postman.com/) — criação e execução das coleções de testes
- [Newman](https://github.com/postmanlabs/newman) — execução headless via linha de comando
- [newman-reporter-htmlextra](https://github.com/DannyDainton/newman-reporter-htmlextra) — geração de relatórios HTML detalhados
- Node.js v20
- SQL Server (via Docker)

---

## Estrutura do projeto

```
accountgo-api-tests/
├── collections/
│   ├── financeiro.json
│   └── vendas.json
├── environments/
│   └── local.json
└── README.md
```

---

## Cobertura de testes

### Comum
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | Termos de pagamento | Lista termos de pagamento |
| GET | Medidas | Lista unidades de medida |

### Financeiro
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| POST | Login | Autenticação com credenciais válidas |
| POST | Login com senha errada | Cenário negativo — credencial inválida |
| POST | Login com email inexistente | Cenário negativo — usuário não cadastrado |
| GET | Lista o Plano de Contas | Listagem completa do plano de contas |
| GET | Lista todos os Lançamentos Contábeis | Listagem de lançamentos |
| GET | Busca um Lançamento específico | Busca por ID válido |
| GET | Buscar lançamento com ID inexistente | Cenário negativo |
| POST | Criar ou Editar um Lançamento | Criação de lançamento válido |
| POST | Criar lançamento com contas duplicadas | Cenário negativo |
| POST | Criar lançamento sem linhas | Cenário negativo |
| POST | Criar lançamento sem data | Cenário negativo |
| POST | Criar lançamento com valor zerado | Cenário negativo |
| POST | Criar lançamento com valor negativo | Cenário negativo |
| POST | Criar lançamento com DrCr inválido | Cenário negativo |
| POST | Criar lançamento com body vazio | Cenário negativo |
| POST | Postar lançamento já postado | Cenário negativo |
| GET | Lista Contas Bancárias | Listagem de contas bancárias |
| GET | Razão Geral | Relatório financeiro |
| GET | Balancete | Relatório financeiro |
| GET | Balanço Patrimonial | Relatório financeiro |
| GET | DRE — Demonstração de Resultado | Relatório financeiro |

#### Fluxo Completo (encadeado)
| Step | Método | Descrição |
|------|--------|-----------|
| 1 | POST | Criar Lançamento para Postar |
| 2 | GET | Buscar Último Lançamento |
| 3 | GET | Verificar Lançamento Não Postado |
| 4 | POST | Postar Lançamento |
| 5 | GET | Confirmar Lançamento Postado |

### Inventário
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | Itens | Lista itens do inventário |

### Vendas
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | Clientes | Lista clientes cadastrados |

#### Fluxo de Vendas (encadeado — ciclo completo)
| Step | Método | Descrição |
|------|--------|-----------|
| 1 | POST | Criar Cotação |
| 2 | GET | Buscar Última Cotação |
| 3 | POST | Converter Cotação em Pedido |
| 4 | GET | Buscar Último Pedido |
| 5 | POST | Criar Nota Fiscal do Pedido |
| 6 | GET | Buscar Última Nota Fiscal |
| 7 | POST | Postar Nota Fiscal |
| 8 | GET | Confirmar Nota Fiscal Postada |
| 9 | POST | Registrar Recebimento |
| 10 | GET | Buscar Último Recebimento |
| 11 | POST | Alocar Recebimento na Nota Fiscal |

---

## Total de testes

| Módulo | Quantidade |
|--------|------------|
| Comum | 2 |
| Financeiro | 25 |
| Inventário | 1 |
| Vendas | 13 |
| **Total** | **41** |

---

## Como executar

### Pré-requisitos

- Node.js v20+
- Newman instalado globalmente:
```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```
- API do AccountGo rodando localmente
- SQL Server ativo via Docker

### Executar uma coleção

```bash
newman run collections/financeiro.json \
  -e environments/local.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/financeiro.html
```

```bash
newman run collections/vendas.json \
  -e environments/local.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/vendas.html
```

---

## Ambiente local

O arquivo `environments/local.json` contém as variáveis de ambiente necessárias para execução local, como URL base e token de autenticação gerado dinamicamente via script no pré-request.

---

## Sobre o projeto

Este projeto foi desenvolvido como parte de um portfólio pessoal de QA, com foco em demonstrar:

- Estratégia de testes para APIs REST
- Cobertura de cenários positivos e negativos
- Encadeamento de requisições com variáveis de ambiente
- Execução headless com Newman
- Geração de relatórios HTML profissionais
- Testes de performance via Postman
