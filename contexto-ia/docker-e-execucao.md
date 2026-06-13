# Docker e execucao

## Compose da raiz

Arquivo:

- `docker-compose.yml`

Sobe a aplicacao completa:

- `db`: PostgreSQL;
- `app`: backend Spring Boot;
- `frontend`: frontend React/Vite.

Comando na raiz:

```bash
docker compose up -d
```

Com rebuild:

```bash
docker compose up -d --build
```

Parar:

```bash
docker compose down
```

Parar e remover volumes:

```bash
docker compose down -v
```

## Compose do backend

Arquivo:

- `backend/docker-compose.yml`

Sobe apenas:

- `db`;
- `app`.

Comando:

```bash
cd backend
docker compose up -d
```

## Variaveis de ambiente esperadas

As variaveis devem ficar em `.env`, mas esse arquivo nao deve ser compartilhado com IA externa.

Variaveis principais:

```env
POSTGRES_VERSION=15
DB_NAME=db
DB_USER=postgres
DB_PASSWORD=postgres
DB_PORT_EXTERNAL=5434
DB_PORT_INTERNAL=5432

APP_PORT=8080
DDL_AUTO=update
JWT_SECRET=defina-um-segredo-seguro
JWT_EXPIRATION=36000000
FRONTEND_URL=http://localhost:5173
LINK_CADASTRO_VALIDADE_MAXIMA_MINUTOS=43200

MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=seu-email@example.com
MAIL_PASSWORD=sua-senha-ou-app-password

FRONTEND_PORT=5173
VITE_API_PROXY_TARGET=http://app:8080
```

## Backend local

O backend usa Maven:

```bash
cd backend
mvn spring-boot:run
```

Para testes:

```bash
cd backend
mvn test
```

## Frontend local

Instalar dependencias:

```bash
cd frontend
npm install
```

Rodar dev server:

```bash
npm run dev
```

Build:

```bash
npm run build
```

Lint:

```bash
npm run lint
```

## Portas

- Backend: `8080`.
- Frontend local via npm: `3000`.
- Frontend via Docker Compose da raiz: definido por `FRONTEND_PORT`, exemplo `5173`.
- Postgres externo: definido por `DB_PORT_EXTERNAL`, exemplo `5434`.

## Arquivos que nao devem ser exportados

- `.env`
- `.git/`
- `frontend/node_modules/`
- `backend/target/`
- `frontend/build/`
- logs, caches e arquivos temporarios.
