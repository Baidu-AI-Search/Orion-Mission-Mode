# TaskFlow API

A lightweight **Task Management REST API** built with TypeScript, Express, Prisma, and PostgreSQL. TaskFlow supports user registration and login (JWT), full CRUD operations on personal tasks, and a signed webhook endpoint for externally triggered task completion. It also integrates with Redis (via `ioredis`) for future caching / session / queue use cases.

## Tech Stack

- **Language:** TypeScript 5.x
- **Runtime:** Node.js
- **Web Framework:** Express 4.x
- **ORM & Database:** Prisma 5.x with PostgreSQL
- **Authentication:** JSON Web Tokens (`jsonwebtoken`) + password hashing via `bcrypt`
- **Cache / Queue:** Redis (`ioredis`)
- **Configuration:** `dotenv`
- **Development Tooling:** `ts-node-dev`, `tsc`, `eslint`, `jest`

## Getting Started

### Prerequisites

- **Node.js** (v18 or later recommended)
- **npm** (or another package manager)
- **PostgreSQL** (running locally or accessible via connection URL)
- **Redis** (running locally or accessible via connection URL — required at runtime by the `ioredis` client configuration)

### Installation

1. Clone the repository and navigate into the project folder:

   ```bash
   git clone <repo-url> taskflow-api
   cd taskflow-api
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create an environment file from the example:

   ```bash
   cp .env.example .env
   ```

   Edit `.env` and fill in your actual database credentials, a strong JWT secret, and webhook secret. See the [Environment Variables](#environment-variables) section below for details.

4. Generate the Prisma client:

   ```bash
   npx prisma generate
   ```

### Database Setup

The project uses PostgreSQL through Prisma. Apply all pending migrations to create the tables (`User`, `Task`) in your database:

```bash
npm run db:migrate
```

_(Optional)_ If the project ships a seed file at `prisma/seed.ts`, you can populate the database with starter data:

```bash
npm run db:seed
```

### Running Locally

Start the development server with hot reload:

```bash
npm run dev
```

The server will start on the port specified in your `.env` file (default `3000`). You can verify it is running by hitting the health endpoint:

```bash
curl http://localhost:3000/health
# -> {"status":"ok"}
```

For production, build and start the compiled output:

```bash
npm run build
npm start
```

## Environment Variables

Copy `.env.example` to `.env` and configure the following variables:

| Variable         | Required | Default                                   | Description                                                              |
| ---------------- | :------: | ----------------------------------------- | ------------------------------------------------------------------------ |
| `PORT`           |    No    | `3000`                                    | TCP port the Express server listens on.                                  |
| `DATABASE_URL`   |   Yes    | `postgresql://localhost:5432/taskflow`    | PostgreSQL connection string used by Prisma (`postgresql://user:pwd@host:port/db`). |
| `JWT_SECRET`     |   Yes    | `change-me` (development only!)           | Secret used to sign and verify JWT access tokens. **Must be changed in production.** |
| `REDIS_URL`      |    No    | `redis://localhost:6379`                  | Redis connection string used by the `ioredis` client.                    |
| `WEBHOOK_SECRET` |   Yes    | _(empty)_                                 | HMAC-SHA256 secret used to verify signatures on incoming webhook calls.  |

## API Endpoints

All endpoints (except health, registration, login, and webhooks) require a valid JWT in the `Authorization: Bearer <token>` header.

### Health

| Method | Route      | Description                          |
| ------ | ---------- | ------------------------------------ |
| GET    | `/health`  | Liveness check; returns `{status:"ok"}`. |

### Auth — `/api/auth`

| Method | Route       | Description                                                                  |
| ------ | ----------- | ---------------------------------------------------------------------------- |
| POST   | `/register` | Register a new user. Body: `{ email, password, name }`. Returns the new user's `id` and `email`. |
| POST   | `/login`    | Authenticate with `{ email, password }`. Returns a JWT `{ token }` valid for 7 days. |

### Tasks — `/api/tasks` (authenticated)

All routes are scoped to the authenticated user; users can only see / modify their own tasks.

| Method | Route        | Description                                                                                         |
| ------ | ------------ | --------------------------------------------------------------------------------------------------- |
| GET    | `/`          | List all tasks belonging to the current user.                                                       |
| POST   | `/`          | Create a new task. Body: `{ title, description?, dueDate? }`. Returns the created task.             |
| PATCH  | `/:id`       | Update an existing task (any subset of task fields). Returns the updated task.                      |
| DELETE | `/:id`       | Delete a task. Responds with `204 No Content`.                                                      |

Task objects have the following shape:

```json
{
  "id": "uuid",
  "title": "string",
  "description": "string | null",
  "status": "pending | done",
  "dueDate": "ISO-8601 | null",
  "completedAt": "ISO-8601 | null",
  "userId": "uuid",
  "createdAt": "ISO-8601",
  "updatedAt": "ISO-8601"
}
```

### Webhooks — `/api/webhooks`

| Method | Route            | Description                                                                                                                                                         |
| ------ | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| POST   | `/task-complete` | Receives an external completion event. Verifies the `X-Webhook-Signature` header using HMAC-SHA256 with `WEBHOOK_SECRET`, then marks the referenced task as `done`. |

Expected webhook body:

```json
{ "taskId": "uuid", "completedAt": "ISO-8601" }
```

The server compares `X-Webhook-Signature` to `HMAC-SHA256(WEBHOOK_SECRET, json(payload))`. Requests with an invalid signature are rejected with `403 Forbidden`.

## Available Scripts

The following scripts are defined in `package.json`:

| Script             | Command                                         | What it does                                                               |
| ------------------ | ----------------------------------------------- | -------------------------------------------------------------------------- |
| `npm run dev`      | `ts-node-dev --respawn src/index.ts`            | Starts the server in development mode with automatic reload on file changes. |
| `npm run build`    | `tsc`                                           | Compiles TypeScript sources from `src/` to `dist/` using `tsconfig.json`.   |
| `npm start`        | `node dist/index.js`                            | Runs the compiled production build. (Run `npm run build` first.)            |
| `npm test`         | `jest`                                          | Runs the Jest test suite.                                                  |
| `npm run lint`     | `eslint src/`                                   | Lints all TypeScript files in `src/` with ESLint.                          |
| `npm run db:migrate` | `prisma migrate dev`                          | Applies pending Prisma migrations against the database in development mode, generating the Prisma client. |
| `npm run db:seed`  | `ts-node prisma/seed.ts`                        | Executes the Prisma seed script to populate the database with initial data. |

## Project Structure

```
taskflow-api/
├── prisma/
│   └── schema.prisma        # Prisma models (User, Task) and datasource config
├── src/
│   ├── config.ts            # Loads and exports environment configuration
│   ├── index.ts             # Express app entrypoint: mounts routers + starts server
│   └── routes/
│       ├── auth.ts          # /api/auth — register & login
│       ├── tasks.ts         # /api/tasks — CRUD for tasks (auth-protected)
│       └── webhooks.ts      # /api/webhooks — signed task-complete webhook
├── .env.example             # Example environment variables
├── package.json
└── tsconfig.json
```

## Data Models (Prisma)

- **User** — `id (uuid, pk)`, `email (unique)`, `password (bcrypt hash)`, `name`, `createdAt`, and a one-to-many relation to `Task`.
- **Task** — `id (uuid, pk)`, `title`, `description?`, `status` (defaults to `"pending"`), `dueDate?`, `completedAt?`, `userId (fk)`, `createdAt`, `updatedAt`.

## License

This project is licensed under the terms of the **MIT** license. See the `license` field in `package.json` for details.
