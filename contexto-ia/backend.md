# Contexto do backend

## Localizacao

O backend fica em `backend/`.

Arquivos principais:

- `backend/pom.xml`
- `backend/src/main/java/br/edu/udf/mione/MioneApplication.java`
- `backend/src/main/resources/application.properties`
- `backend/init/01_schema.sql`
- `backend/init/02_dados_iniciais.sql`
- `backend/Dockerfile`
- `backend/docker-compose.yml`
- `backend/README.md`

## Arquitetura

O backend segue uma arquitetura em camadas:

```text
Controller REST
        |
        v
Service interface
        |
        v
Service implementation
        |
        v
Repository Spring Data JPA
        |
        v
Entities JPA / PostgreSQL
```

O documento completo esta em `doc/arquitetura-backend.md`.

## Pacotes de `backend/src/main/java/br/edu/udf/mione`

- `controller`
  - entrada HTTP da API REST;
  - controllers por perfil ou area: `AuthController`, `PublicAuthController`, `PublicController`, `AlunoController`, `MonitorController`, `CoordenadorController`, `AdminController`.

- `service`
  - interfaces dos casos de uso;
  - contratos como `UsuarioService`, `AtendimentoService`, `HorarioMonitoriaService`, `DisponibilidadeRecorrenteService`.

- `service/impl`
  - implementacoes dos casos de uso;
  - regras de negocio, validacoes, transacoes, envio de email e geracao de links.

- `repository`
  - persistencia via Spring Data JPA;
  - interfaces que estendem `JpaRepository`;
  - consultas derivadas e JPQL com `@Query`.

- `model`
  - entidades JPA e enums do dominio;
  - representa usuarios, perfis, disciplinas, periodos, horarios, atendimentos, status e links.

- `dto`
  - objetos de transporte;
  - inclui DTOs de request, response, usuario autenticado e informacoes de token.

- `security`
  - Spring Security, JWT, filtros, `UserDetailsService`, `PasswordEncoder` e autorizacao por perfil.

- `infra`
  - tratamento global de excecoes, envelopes de resposta, CORS, scheduler e componentes tecnicos.

## Entidades principais

- `Usuario`
- `Perfil`
- `UsuarioPerfil`
- `Disciplina`
- `PeriodoLetivo`
- `DisponibilidadeRecorrente`
- `HorarioMonitoria`
- `Atendimento`
- `StatusAtendimento`
- `StatusHorarioMonitoria`
- `DiaSemana`
- `Link`
- `TipoLink`

## Controllers e bases de rota

Todas as rotas consideram o prefixo global configurado:

```properties
server.servlet.context-path=/api
```

Principais bases:

- `/api/public/auth`
- `/api/auth`
- `/api/public`
- `/api/aluno`
- `/api/monitor`
- `/api/coordenador`
- `/api/admin`

Os detalhes ficam em `doc/mapaControllers/`.

## Seguranca

- Autenticacao via Spring Security.
- JWT gerado pelo backend.
- Token enviado ao frontend em cookie `HttpOnly`.
- Aplicacao stateless.
- Autorizacao por `@PreAuthorize` e authorities:
  - `ACESSO_ALUNO`
  - `ACESSO_MONITOR`
  - `ACESSO_COORDENADOR`
  - `ACESSO_ADMIN`

## Banco de dados

Banco: PostgreSQL.

Scripts:

- `backend/init/01_schema.sql`: estrutura principal.
- `backend/init/02_dados_iniciais.sql`: dados iniciais.

Tabelas importantes:

- `usuario`
- `perfil`
- `usuario_perfil`
- `disciplina`
- `periodo_letivo`
- `disponibilidade_recorrente`
- `horario_monitoria`
- `atendimento`
- `status_atendimento`
- `status_horario_monitoria`
- `link`
- `tipo_link`

## Pontos de atencao

- Algumas regras ainda usam `RuntimeException` generica.
- Alguns controllers acessam repository diretamente em pontos especificos.
- Existe arquivo temporario `.SecurityConfig.java.swp` em `security`; deve ser ignorado/removido, nao documentado como arquitetura.
- Ao alterar endpoint, atualizar `doc/mapaControllers/`.
- Ao alterar arquitetura, atualizar `doc/arquitetura-backend.md`.
