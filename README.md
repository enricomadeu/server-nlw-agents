# Server NLW Agents

Um servidor backend desenvolvido em Node.js para processamento de áudio e integração com IA, utilizando Fastify, TypeScript e PostgreSQL com suporte a embeddings vetoriais.

## 🚀 Tecnologias Utilizadas

- **Node.js** com TypeScript (experimental)
- **Fastify** - Framework web rápido e eficiente
- **PostgreSQL** com **pgvector** - Banco de dados com suporte a embeddings
- **Drizzle ORM** - ORM moderno para TypeScript
- **Google Gemini AI** - Processamento de áudio e geração de embeddings
- **Zod** - Validação de esquemas
- **Docker** - Containerização do banco de dados

## 📋 Pré-requisitos

- Node.js 18+ (com suporte ao `--experimental-strip-types`)
- Docker e Docker Compose
- Chave da API do Google Gemini

## 🛠️ Instalação

1. **Clone o repositório:**

```bash
git clone https://github.com/enricomadeu/server-nlw-agents.git
cd server-nlw-agents
```

2. **Instale as dependências:**

```bash
npm install
```

3. **Configure as variáveis de ambiente:**

```bash
cp .env.exemple .env
```

Edite o arquivo `.env` com suas configurações:

```env
PORT=3333
DATABASE_URL=postgresql://docker:docker@localhost:5432/agents
GEMINI_API_KEY=sua_chave_da_api_gemini
```

4. **Inicie o banco de dados:**

```bash
docker-compose up -d
```

5. **Execute as migrações:**

```bash
npm run db:migrate
```

6. **Popule o banco (opcional):**

```bash
npm run db:seed
```

## 🚀 Executando a Aplicação

### Desenvolvimento

```bash
npm run dev
```

### Produção

```bash
npm start
```

### Scripts Disponíveis

- `npm run dev` - Executa em modo desenvolvimento com hot reload
- `npm start` - Executa em modo produção
- `npm run db:generate` - Gera migrações do Drizzle
- `npm run db:migrate` - Executa migrações pendentes
- `npm run db:seed` - Popula o banco com dados iniciais

## 📡 API Endpoints

### Health Check

```http
GET /health
```

Retorna o status da aplicação.

### Salas (Rooms)

#### Listar Salas

```http
GET /rooms
```

#### Criar Sala

```http
POST /rooms
Content-Type: application/json

{
  "name": "Nome da Sala",
  "description": "Descrição opcional"
}
```

#### Obter Perguntas de uma Sala

```http
GET /rooms/:roomId/questions
```

### Perguntas

#### Criar Pergunta

```http
POST /rooms/:roomId/questions
Content-Type: application/json

{
  "question": "Sua pergunta aqui"
}
```

### Upload de Áudio

#### Enviar Áudio para Processamento

```http
POST /rooms/:roomId/audio
Content-Type: multipart/form-data

{
  "audio": [arquivo de áudio]
}
```

Este endpoint:

- Aceita arquivos de áudio em vários formatos
- Transcreve o áudio usando Google Gemini
- Gera embeddings para busca semântica
- Armazena os dados no banco para consultas futuras

## 🗃️ Estrutura do Banco de Dados

### Tabelas Principais

#### `rooms`

- `id` - UUID (Primary Key)
- `name` - Nome da sala
- `description` - Descrição opcional
- `created_at` - Data de criação

#### `questions`

- `id` - UUID (Primary Key)
- `room_id` - Referência para a sala
- `question` - Texto da pergunta
- `created_at` - Data de criação

#### `audio_chunks`

- `id` - UUID (Primary Key)
- `room_id` - Referência para a sala
- `transcription` - Transcrição do áudio
- `embeddings` - Vetores para busca semântica (pgvector)
- `created_at` - Data de criação

## 🏗️ Arquitetura do Projeto

```
src/
├── db/
│   ├── connection.ts          # Configuração da conexão
│   ├── seed.ts               # Scripts de população
│   ├── migrations/           # Migrações do banco
│   └── schema/              # Esquemas das tabelas
│       ├── rooms.ts
│       ├── questions.ts
│       ├── audio-chunks.ts
│       └── index.ts
├── http/
│   └── routes/              # Definição das rotas
│       ├── create-room.ts
│       ├── get-rooms.ts
│       ├── get-room-questions.ts
│       ├── create-question.ts
│       └── upload-audio.ts
├── services/
│   └── gemini.ts            # Integração com Google Gemini
├── env.ts                   # Validação de variáveis de ambiente
└── server.ts               # Configuração do servidor
```

## 🤖 Integração com IA

O projeto utiliza o **Google Gemini** para:

- **Transcrição de Áudio**: Converte arquivos de áudio em texto
- **Geração de Embeddings**: Cria representações vetoriais para busca semântica
- **Processamento Inteligente**: Permite análise avançada do conteúdo

### Funcionalidades de IA

1. **Upload de Áudio**: Aceita arquivos de áudio e os processa automaticamente
2. **Transcrição Automática**: Converte fala em texto usando modelos avançados
3. **Busca Semântica**: Utiliza embeddings para encontrar conteúdo relacionado
4. **Análise de Contexto**: Processa e relaciona informações de diferentes fontes

## 🐳 Docker

O projeto inclui configuração Docker para o banco de dados PostgreSQL com extensão pgvector:

```yaml
# docker-compose.yml
services:
  nlw-agents-pg:
    image: pgvector/pgvector:pg17
    environment:
      POSTGRES_USER: docker
      POSTGRES_PASSWORD: docker
      POSTGRES_DB: agents
    ports:
      - "5432:5432"
```

## 🔧 Configurações Avançadas

### TypeScript Experimental

O projeto utiliza o flag `--experimental-strip-types` do Node.js, permitindo executar TypeScript diretamente sem transpilação.

### Validação com Zod

Todas as entradas da API são validadas usando Zod para garantir type safety e consistência.

### ORM Drizzle

Utiliza Drizzle ORM para:

- Type-safe database queries
- Migrações automáticas
- Excelente integração com TypeScript

## 🚨 Variáveis de Ambiente

| Variável         | Descrição                  | Padrão |
| ---------------- | -------------------------- | ------ |
| `PORT`           | Porta do servidor          | `3333` |
| `DATABASE_URL`   | URL de conexão PostgreSQL  | -      |
| `GEMINI_API_KEY` | Chave da API Google Gemini | -      |

## 📚 Dependências Principais

### Produção

- `fastify` - Framework web
- `@fastify/cors` - Middleware CORS
- `@fastify/multipart` - Upload de arquivos
- `drizzle-orm` - ORM
- `postgres` - Driver PostgreSQL
- `@google/genai` - SDK Google Gemini
- `zod` - Validação de esquemas

### Desenvolvimento

- `drizzle-kit` - CLI do Drizzle
- `@biomejs/biome` - Linter e formatter
- `typescript` - Suporte TypeScript
- `ultracite` - Ferramentas de desenvolvimento

## 🤝 Contribuindo

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📝 Licença

Este projeto está sob a licença ISC.

## 👨‍💻 Autor

**Enrico Fernandes**

---

## 🔗 Links Úteis

- [Fastify Documentation](https://www.fastify.io/)
- [Drizzle ORM](https://orm.drizzle.team/)
- [Google Gemini AI](https://ai.google.dev/)
- [pgvector](https://github.com/pgvector/pgvector)

---

**Desenvolvido com ❤️ para o Next Level Week**
