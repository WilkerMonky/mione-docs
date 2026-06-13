# Contexto geral do projeto

## Produto

Mione e uma plataforma de monitorias academicas desenvolvida no contexto Labtech - UDF. O sistema centraliza cadastro de usuarios, perfis de acesso, disciplinas, disponibilidades de monitores, horarios de monitoria e atendimentos agendados por alunos.

## Perfis principais

- Aluno:
  - consulta horarios disponiveis;
  - agenda atendimentos;
  - visualiza e cancela seus agendamentos.

- Monitor:
  - cria horarios de monitoria;
  - cadastra disponibilidades recorrentes;
  - bloqueia ou libera horarios;
  - visualiza e cancela atendimentos vinculados a seus horarios.

- Coordenador:
  - cadastra e aprova alunos e monitores;
  - gera links de cadastro;
  - gerencia disciplinas.

- Admin:
  - possui acesso administrativo ampliado;
  - gerencia coordenadores e administradores;
  - pode acessar fluxos de coordenacao.

## Fluxo funcional resumido

```text
Coordenador/Admin cria ou convida usuarios
        |
        v
Usuario acessa por login ou link de cadastro
        |
        v
Monitor cadastra horarios ou disponibilidades recorrentes
        |
        v
Sistema gera slots de monitoria
        |
        v
Aluno consulta horarios disponiveis
        |
        v
Aluno agenda atendimento
        |
        v
Aluno/Monitor acompanha ou cancela atendimento
```

## Stack principal

Backend:

- Java 17;
- Spring Boot;
- Spring Web;
- Spring Data JPA/Hibernate;
- Spring Security;
- JWT;
- PostgreSQL;
- Spring Mail;
- Thymeleaf;
- Maven.

Frontend:

- React;
- Vite;
- TypeScript;
- React Router;
- TanStack React Query;
- Zustand;
- Axios;
- Tailwind CSS;
- Headless UI;
- Lucide React.

Infraestrutura:

- Docker;
- Docker Compose;
- Postgres 15;
- scripts SQL de inicializacao em `backend/init/`.

## Estrutura de alto nivel

```text
Projeto/
  backend/
    src/
    init/
    pom.xml
    Dockerfile
    docker-compose.yml
    README.md
  frontend/
    src/
    package.json
    vite.config.ts
    Dockerfile
    README.md
  doc/
    README.md
    arquitetura-backend.md
    arquitetura-frontend.md
    mapaControllers/
    finalizado/
    contexto-ia/
  docker-compose.yml
```

## Documentos principais para entender o projeto

- `doc/README.md`: indice geral da documentacao.
- `doc/arquitetura-backend.md`: mapa detalhado do backend.
- `doc/arquitetura-frontend.md`: mapa detalhado do frontend.
- `doc/mapaControllers/README.md`: indice de endpoints.
- `backend/README.md`: execucao do backend.
- `frontend/README.md`: execucao do frontend.
