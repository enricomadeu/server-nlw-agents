# NLW Agents

Backend do projeto **NLW Agents** desenvolvido durante o evento da Rocketseat. Este projeto é um servidor backend construído com Node.js, Fastify, TypeScript, Drizzle ORM e PostgreSQL, focado em performance, tipagem e boas práticas de organização.

## ✨ Tecnologias Utilizadas

- **Node.js** & **TypeScript** — Plataforma e linguagem principal
- **Fastify** — Framework web rápido e eficiente
- **Zod** — Validação e tipagem de dados
- **Drizzle ORM** — ORM moderno, type safe e focado em SQL
- **PostgreSQL** — Banco de dados relacional
- **Docker** — Facilita o setup do banco de dados para desenvolvimento
- **Drizzle Kit** — Gerenciamento de migrations
- **Biome** — Linter e formatter para o código

## 📁 Estrutura de Pastas

```
src/
  db/
    connection.ts        # Conexão com o banco de dados
    migrations/          # Migrations do banco
    schema/              # Schemas das tabelas
    seed.ts              # Seed de dados
  env.ts                 # Validação e carregamento das variáveis de ambiente
  http/
    routes/              # Rotas HTTP da aplicação
      get-rooms.ts
  server.ts              # Inicialização do servidor Fastify
```

## 🏛️ Padrões de Projeto

- **Validação centralizada**: Todas as variáveis de ambiente e dados de entrada são validados com Zod.
- **Separação de responsabilidades**: Rotas, lógica de banco e configuração estão em módulos separados.
- **Migrations e Seeds**: Gerenciados via Drizzle ORM.
- **Arquitetura modular**: Fácil de escalar e manter.

## ⚙️ Setup e Configuração

### 1. Clone o repositório

```bash
git clone <url-do-repo>
cd server-nlw-agents
```

### 2. Instale as dependências

```bash
npm install
```

### 3. Configure o banco de dados

Suba o banco de dados localmente usando Docker:

```bash
docker-compose up -d
```

As credenciais padrão são:

- **Usuário:** docker
- **Senha:** docker
- **Banco:** agents

### 4. Configure as variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto:

```
PORT=3333
DATABASE_URL=postgresql://docker:docker@localhost:5432/agents
```

### 5. Rode as migrations e seeds

```bash
npm run db:seed
```

### 6. Inicie o servidor em modo desenvolvimento

```bash
npm run dev
```

Acesse: [http://localhost:3333](http://localhost:3333)

## 🛠️ Comandos Úteis

- `npm run dev` — Inicia o servidor em modo desenvolvimento (hot reload)
- `npm start` — Inicia o servidor em modo produção
- `npm run db:seed` — Popula o banco de dados com dados iniciais
- `npx biome check .` — Lint do código
- `npx biome format .` — Formata o código

## 📚 Endpoints

### `GET /health`

Verifica se o servidor está online.

**Resposta:**

```json
"OK"
```

---

### `GET /rooms`

Lista todas as salas cadastradas no banco de dados.

**Resposta:**

```json
[
  {
    "id": "uuid",
    "name": "Nome da sala"
  }
]
```

- **id**: Identificador único da sala (UUID)
- **name**: Nome da sala

## 🧪 Testando a API

Você pode testar as rotas com ferramentas como [Insomnia](https://insomnia.rest/) ou [Thunder Client](https://www.thunderclient.com/), ou usando o arquivo `client.http` incluso no projeto.

## 📝 Sobre

Este projeto foi desenvolvido durante o evento **NLW Agents da Rocketseat**, com o objetivo de demonstrar boas práticas de backend moderno, tipagem, validação e organização de código.
