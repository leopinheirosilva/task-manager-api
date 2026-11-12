# Task Manager API

API REST para gerenciamento de tarefas diárias, desenvolvida com Node.js e JSON Server, estruturada para execução local e deploy serverless na Vercel.

## Tech Stack

- **Back-End:** Node.js
- **API / Mock Engine:** JSON Server (`json-server`)
- **Deploy / Serverless:** Vercel (funções serverless via `vercel.json`)
- **Persistência de Dados:** Arquivo JSON (`db.json`)

## Principais features

- CRUD completo de tarefas (Criação, Leitura, Atualização e Exclusão).
- Suporte a prefixo `/api` via reescrita de rotas (ex.: `/api/tasks` redireciona para `/tasks`).
- Suporte nativo do JSON Server para:
  - Filtros por atributos (ex.: `status`, `time`).
  - Ordenação (`_sort` e `_order`).
  - Paginação (`_page` e `_limit`).
  - Busca textual (`q`).
- Pronto para deploy serverless na Vercel com empacotamento do banco de dados `db.json`.

## Modelo de Dados (Tarefas)

Cada tarefa (`task`) armazenada possui o seguinte formato:

| Campo | Tipo | Descrição | Valores Possíveis / Exemplo |
| :--- | :--- | :--- | :--- |
| `id` | `string` | Identificador único (UUID) | `"14f399cb-969e-4ee8-8eab-b28ede311845"` |
| `title` | `string` | Título da tarefa | `"Estudar FSC"` |
| `description`| `string` | Descrição detalhada da atividade | `"Estudar para a prova de FSC"` |
| `time` | `string` | Período planejado para execução | `"morning"`, `"afternoon"`, `"night"` |
| `status` | `string` | Estado atual da tarefa | `"not_started"`, `"in_progress"`, `"done"` |

## Instalação e Execução Local

### Pré-requisitos

- Node.js (versão 18 ou superior)
- npm ou yarn

### Passo a passo

1. Clone o repositório e acesse a pasta do projeto:

```bash
git clone https://github.com/leopinheirosilva/task-manager-api.git
cd task-manager-api
```

2. Instale as dependências:

```bash
npm install
```

3. Inicie o servidor:

```bash
npm start
```

O servidor estará em execução em `http://localhost:3000`.

## Rotas da API

As requisições podem ser feitas diretamente ou utilizando o prefixo `/api/*`:

| Método | Endpoint | Descrição |
| :--- | :--- | :--- |
| `GET` | `/tasks` ou `/api/tasks` | Lista todas as tarefas |
| `GET` | `/tasks/:id` ou `/api/tasks/:id` | Retorna os detalhes de uma tarefa específica |
| `POST` | `/tasks` ou `/api/tasks` | Cria uma nova tarefa |
| `PUT` | `/tasks/:id` ou `/api/tasks/:id` | Atualiza integralmente uma tarefa |
| `PATCH` | `/tasks/:id` ou `/api/tasks/:id` | Atualiza campos parciais de uma tarefa |
| `DELETE` | `/tasks/:id` ou `/api/tasks/:id` | Remove uma tarefa |

### Exemplos de Consultas (Filtros e Busca)

- **Filtrar por status:**
  ```http
  GET /tasks?status=in_progress
  ```
- **Filtrar por período:**
  ```http
  GET /tasks?time=morning
  ```
- **Busca por texto:**
  ```http
  GET /tasks?q=estudar
  ```
- **Ordenação:**
  ```http
  GET /tasks?_sort=title&_order=asc
  ```

## Estrutura do Projeto

```
task-manager-api/
├── api/
│   └── server.js      # Configuração do JSON Server e entry point para serverless
├── db.json            # Base de dados em formato JSON com as tarefas
├── package.json       # Dependências e scripts da aplicação
├── vercel.json        # Configuração de rotas e funções serverless da Vercel
└── README.md          # Documentação do projeto
```

## Deploy na Vercel

O projeto já está configurado via [vercel.json](file:///home/leonardo/projetos/task-manager-api/vercel.json) para rodar como Serverless Function na Vercel:

1. Instale a CLI da Vercel (opcional):
   ```bash
   npm i -g vercel
   ```
2. Realize o deploy:
   ```bash
   vercel
   ```
> [!NOTE]
> Em ambientes serverless (como na Vercel), o arquivo `db.json` é tratado em modo somente-leitura após o build. Modificações em tempo de execução via `POST`/`PUT`/`DELETE` não serão persistidas de forma durável no disco da função efêmera.
