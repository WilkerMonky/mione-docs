# Inventario de arquivos para contexto

Este inventario orienta quais arquivos sao relevantes para uma IA analisar o projeto.

## Arquivos essenciais da raiz

- `docker-compose.yml`
- `.env.example`, se existir no futuro
- `doc/README.md`

Nao exportar:

- `.env`
- `.git/`

## Backend

Essenciais:

- `backend/pom.xml`
- `backend/README.md`
- `backend/Dockerfile`
- `backend/docker-compose.yml`
- `backend/src/main/java/br/edu/udf/mione/MioneApplication.java`
- `backend/src/main/resources/application.properties`
- `backend/init/01_schema.sql`
- `backend/init/02_dados_iniciais.sql`

Codigo fonte relevante:

- `backend/src/main/java/br/edu/udf/mione/controller/`
- `backend/src/main/java/br/edu/udf/mione/service/`
- `backend/src/main/java/br/edu/udf/mione/service/impl/`
- `backend/src/main/java/br/edu/udf/mione/repository/`
- `backend/src/main/java/br/edu/udf/mione/model/`
- `backend/src/main/java/br/edu/udf/mione/dto/`
- `backend/src/main/java/br/edu/udf/mione/security/`
- `backend/src/main/java/br/edu/udf/mione/infra/`

Testes:

- `backend/src/test/java/`
- `backend/src/test/resources/`

Nao exportar:

- `backend/target/`
- `backend/.git/`
- arquivos `.swp`;
- credenciais reais.

## Frontend

Essenciais:

- `frontend/package.json`
- `frontend/package-lock.json`
- `frontend/README.md`
- `frontend/Dockerfile`
- `frontend/vite.config.ts`
- `frontend/tsconfig.json`
- `frontend/tailwind.config.js`
- `frontend/src/main.tsx`

Codigo fonte relevante:

- `frontend/src/app/`
- `frontend/src/pages/`
- `frontend/src/widgets/`
- `frontend/src/features/`
- `frontend/src/entities/`
- `frontend/src/shared/`

Nao exportar:

- `frontend/node_modules/`
- `frontend/build/`
- `frontend/.git/`
- caches e artefatos de ferramenta.

## Documentacao

Essenciais:

- `doc/README.md`
- `doc/arquitetura-backend.md`
- `doc/arquitetura-frontend.md`
- `doc/mapaControllers/`
- `doc/contexto-ia/`
- `doc/CARTA.md`
- `doc/melhorias-e-funcionalidades..md`

Formais:

- `doc/finalizado/`

Historicos:

- `doc/Documentacao IBM base/`

## Comando sugerido para criar um pacote zip limpo

Use apenas se precisar enviar um pacote de arquivos para outra ferramenta:

```bash
zip -r mione-contexto-ia.zip \
  docker-compose.yml \
  backend/src \
  backend/init \
  backend/pom.xml \
  backend/Dockerfile \
  backend/docker-compose.yml \
  backend/README.md \
  frontend/src \
  frontend/package.json \
  frontend/package-lock.json \
  frontend/vite.config.ts \
  frontend/tsconfig.json \
  frontend/tailwind.config.js \
  frontend/Dockerfile \
  frontend/README.md \
  doc \
  -x "*/.git/*" "*/node_modules/*" "*/target/*" "*/build/*" ".env" "*.swp"
```

Antes de compartilhar, revise o zip para confirmar que nao ha segredo.
