# Arquitetura do Backend

Este documento mapeia a arquitetura do backend do projeto Mione, localizado em `backend/`, com foco na estrutura de `src`, no fluxo entre camadas e no padrão arquitetural que melhor descreve a implementação atual.

## Resumo executivo

O backend é uma aplicação Java com Spring Boot, exposta como API REST sob o context path `/api`. O projeto usa Spring Web, Spring Data JPA, Spring Security, JWT, PostgreSQL, validação, envio de e-mail com Spring Mail, templates Thymeleaf e agendamentos com Spring Scheduling.

A arquitetura implementada se encaixa principalmente em uma arquitetura em camadas, também conhecida como layered architecture ou n-tier architecture, com uso explícito dos padrões Service Layer, Repository e DTO. Na prática, o fluxo dominante é:

```text
Cliente HTTP / Frontend
        |
        v
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

Além desse fluxo principal, existem camadas transversais para segurança, tratamento global de erros, resposta padronizada, CORS, e execução agendada.

## Enquadramento arquitetural

### Padrão principal: arquitetura em camadas

O projeto organiza responsabilidades por pacotes técnicos:

- `controller`: entrada HTTP e adaptação da API.
- `service` e `service/impl`: casos de uso, regras de negócio e transações.
- `repository`: acesso a dados via Spring Data JPA.
- `model`: entidades persistidas e enums de domínio.
- `dto`: contratos de entrada, saída e transporte de dados.
- `security`: autenticação, autorização e JWT.
- `infra`: infraestrutura transversal da aplicação.

Essa estrutura corresponde a uma arquitetura em camadas porque cada grupo tem uma responsabilidade predominante e as dependências descem em direção à persistência. Controllers chamam services; services coordenam regras e repositories; repositories acessam entidades e banco.

### Padrões de design observados

**Service Layer**

O pacote `service` define interfaces como `UsuarioService`, `AtendimentoService`, `HorarioMonitoriaService` e `DisponibilidadeRecorrenteService`. As implementações em `service/impl` concentram regras como cadastro de usuários por perfil, geração de links, agendamento/cancelamento de atendimentos, criação de horários, geração de slots recorrentes e transações com `@Transactional`.

Esse desenho se encaixa no padrão Service Layer: uma camada de serviços que define a fronteira da aplicação, oferece operações de negócio para os clientes internos e coordena respostas, regras e transações.

**Repository / Data Access Layer**

O pacote `repository` usa interfaces que estendem `JpaRepository`, por exemplo `UsuarioRepository`, `HorarioMonitoriaRepository`, `AtendimentoRepository` e `DisponibilidadeRecorrenteRepository`. Elas encapsulam consultas derivadas e JPQL com `@Query`, escondendo detalhes de persistência dos services.

Esse desenho se encaixa no padrão Repository e atua como Data Access Layer da aplicação.

**DTO**

O pacote `dto` contém objetos de transporte usados nas bordas da API e em projeções de consulta. Há DTOs de request, response e segurança. Isso reduz o acoplamento entre payload HTTP e entidades JPA.

**MVC/REST com controllers anotados**

Os controllers usam `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `ResponseEntity` e DTOs. Eles fazem adaptação HTTP e delegam a lógica principal aos services.

**Segurança stateless com JWT**

O pacote `security` configura Spring Security com sessão stateless, filtro JWT, `UserDetailsService`, `PasswordEncoder`, autorização por `@PreAuthorize` e authorities como `ACESSO_ALUNO`, `ACESSO_MONITOR`, `ACESSO_COORDENADOR` e `ACESSO_ADMIN`.

### Observação importante

O projeto não segue Clean Architecture ou Hexagonal Architecture de forma estrita. Ele tem interfaces de serviço, mas as regras de negócio conhecem diretamente entidades JPA, repositories do Spring Data e detalhes do framework. Isso é normal em aplicações Spring Boot em camadas, mas significa que o domínio não está isolado de infraestrutura como em uma arquitetura hexagonal ou clean mais rigorosa.

## Stack e infraestrutura de execução

Principais pontos identificados:

- Aplicação principal: `MioneApplication`, com `@SpringBootApplication`, `@EnableAsync` e `@EnableScheduling`.
- Porta padrão: `8080`.
- Context path: `/api`.
- Banco: PostgreSQL.
- ORM: Spring Data JPA / Hibernate.
- Segurança: Spring Security + JWT armazenado em cookie `HttpOnly`.
- E-mail: Spring Mail + templates HTML em Thymeleaf.
- Docker: `backend/docker-compose.yml` sobe `db` e `app`; o compose da raiz sobe a aplicação completa.
- Inicialização de banco: scripts em `backend/init/`.

## Mapa dos domínios principais

O domínio modelado é um sistema de monitorias acadêmicas. As entidades principais são:

- `Usuario`: pessoa usuária do sistema.
- `Perfil` e `UsuarioPerfil`: papéis de acesso e vínculo usuário-perfil.
- `Disciplina`: disciplina monitorada.
- `PeriodoLetivo`: semestre/ano ativo.
- `DisponibilidadeRecorrente`: disponibilidade semanal cadastrada por monitor.
- `HorarioMonitoria`: slot concreto de monitoria gerado manualmente ou por recorrência.
- `Atendimento`: agendamento de aluno em um horário de monitoria.
- `StatusAtendimento` e `StatusHorarioMonitoria`: tabelas de domínio para estados.
- `Link` e `TipoLink`: links dinâmicos para cadastro e recuperação de senha.
- `DiaSemana`: tabela de apoio para recorrência semanal.

O fluxo funcional mais importante é:

```text
Coordenador/Admin cadastra usuários, disciplinas e gera links
        |
Monitor cria horários ou disponibilidades recorrentes
        |
Scheduler gera slots futuros a partir das disponibilidades
        |
Aluno consulta horários disponíveis e agenda atendimento
        |
Aluno ou monitor cancela atendimento conforme regras do service
```

## Mapa dos diretórios de `backend/src`

### `src/main/java/br/edu/udf/mione`

Pacote raiz da aplicação. Contém `MioneApplication`, classe que inicia o Spring Boot e habilita recursos globais:

- `@SpringBootApplication`: bootstrap da aplicação e component scan.
- `@EnableAsync`: suporte a execução assíncrona.
- `@EnableScheduling`: suporte a tarefas agendadas.

### `src/main/java/br/edu/udf/mione/controller`

Camada de entrada HTTP da API REST. Armazena controllers por perfil ou área funcional:

- `PublicAuthController`: login, recuperação/redefinição de senha e validação de tokens públicos.
- `AuthController`: logout, troca de perfil, perfis disponíveis e dados do usuário autenticado.
- `PublicController`: endpoints públicos de cadastro por link.
- `AlunoController`: operações do aluno, como agendar, listar e cancelar atendimentos.
- `MonitorController`: operações do monitor, como criar/bloquear/liberar horários, gerir disponibilidade recorrente e listar atendimentos.
- `CoordenadorController`: gestão de alunos, monitores, disciplinas e links de cadastro.
- `AdminController`: gestão de coordenadores e administradores.

Responsabilidade correta deste pacote: receber requisições, extrair parâmetros/DTOs, aplicar restrições de endpoint, chamar services e montar `ResponseEntity` com `ApiResponse` ou erro.

### `src/main/java/br/edu/udf/mione/dto`

Objetos de transporte de dados. Evitam expor diretamente todas as entidades JPA nos contratos externos e representam payloads específicos da API ou da segurança.

Arquivos diretamente em `dto`:

- `CurrentUserDto`: dados do usuário autenticado retornados ao frontend.
- `UsuarioSecurityDto`: implementação de `UserDetails` usada pelo Spring Security.
- `UsuarioSimplesDto`: resumo de usuário usado em respostas de horários.
- `InformacoesTokenDto`: dados extraídos de tokens de link.
- `EmailMonitoriaDTO`: estrutura auxiliar para e-mails.

### `src/main/java/br/edu/udf/mione/dto/request`

DTOs de entrada da API. Representam corpos de requisição ou dados enviados pelo cliente:

- Autenticação e senha: `AuthRequestDto`, `LoginRequest`, `SenhaDto`, `TrocaPerfilRequestDto`.
- Usuário/perfil: `UsuarioRequestDto`, `AtualizarMeuPerfilRequestDto`.
- Monitoria/agenda: `HorarioRequestDto`, `DisponibilidadeRecorrenteRequestDTO`, `AtendimentoRequestDTO`, `AgendamentoRequest`, `PresencaRequest`.

Responsabilidade correta deste subpacote: armazenar contratos de entrada, não regras de negócio.

### `src/main/java/br/edu/udf/mione/dto/response`

DTOs de saída da API e projeções de consulta:

- `UsuarioResponseDto`: resposta segura de usuário, sem senha.
- `HorarioMonitoriaResponseDto`: dados de slots de monitoria.
- `AtendimentoResponseDto`: histórico e estado de atendimentos.
- `DisponibilidadeRecorrenteResponseDTO`: retorno de disponibilidades.
- `MonitorDaDisciplinaDto`: projeção de monitores por disciplina.

Responsabilidade correta deste subpacote: definir o formato de resposta para o frontend e carregar apenas os campos necessários.

### `src/main/java/br/edu/udf/mione/model`

Camada de modelo persistente. Contém entidades JPA e enums ligados ao domínio.

Entidades JPA:

- `Usuario`, `Perfil`, `UsuarioPerfil`.
- `Disciplina`, `PeriodoLetivo`, `DiaSemana`.
- `DisponibilidadeRecorrente`, `HorarioMonitoria`, `Atendimento`.
- `StatusAtendimento`, `StatusHorarioMonitoria`.
- `TipoLink`, `Link`.

Enums de domínio:

- `PerfilEnum`.
- `StatusAtendimentoEnum`.
- `StatusHorarioMonitoriaEnum`.
- `UrlLink`.

Responsabilidade correta deste pacote: representar tabelas, relacionamentos e conceitos persistidos do domínio. Hoje as entidades são majoritariamente modelos de dados; a maior parte das regras fica nos services.

### `src/main/java/br/edu/udf/mione/repository`

Camada de persistência. Contém interfaces Spring Data JPA que estendem `JpaRepository` e concentram consultas ao banco.

Repositories principais:

- `UsuarioRepository`, `PerfilRepository`, `UsuarioPerfilRepository`.
- `DisciplinaRepository`, `PeriodoLetivoRepository`, `DiaSemanaRepository`.
- `DisponibilidadeRecorrenteRepository`, `HorarioMonitoriaRepository`, `AtendimentoRepository`.
- `StatusAtendimentoRepository`, `StatusHorarioMonitoriaRepository`.
- `LinkRepository`, `TipoLinkRepository`.

Responsabilidade correta deste pacote: CRUD, consultas derivadas, JPQL com `@Query`, updates com `@Modifying` e projeções DTO quando útil. Regras de negócio devem permanecer nos services, não nos repositories.

### `src/main/java/br/edu/udf/mione/service`

Contratos dos serviços de aplicação. Contém interfaces que descrevem operações disponíveis para cada área:

- `UsuarioService`: usuários, perfis, cadastro por link e recuperação de senha.
- `AtendimentoService`: agendamento, cancelamento e histórico.
- `HorarioMonitoriaService`: criação, bloqueio, liberação, reserva e listagem de horários.
- `DisponibilidadeRecorrenteService`: criação e geração de slots recorrentes.
- `DisciplinaService`, `PeriodoLetivoService`, `DiaSemanaService`.
- `StatusAtendimentoService`, `StatusHorarioMonitoriaService`.
- `EmailService`, `GeradorDeLinkService`.

Responsabilidade correta deste pacote: declarar os casos de uso sem amarrar callers à implementação concreta.

### `src/main/java/br/edu/udf/mione/service/impl`

Implementações reais dos serviços. É onde está a maior parte da lógica de negócio e orquestração:

- Validações de cadastro e atualização.
- Hash de senha via `PasswordEncoder`.
- Vínculo, ativação e desativação de perfis.
- Agendamento e cancelamento de atendimentos.
- Criação e mudança de status de horários.
- Geração de slots recorrentes.
- Geração e validação de links dinâmicos.
- Envio de e-mails.
- Transações com `@Transactional`.

Exemplo de fluxo interno:

```text
MonitorController.criarDisponibilidadeRecorrente
        |
        v
DisponibilidadeRecorrenteServiceImpl.criar
        |
        |-- valida usuário, disciplina, dia e período via outros services
        |-- checa conflito via DisponibilidadeRecorrenteRepository
        |-- salva DisponibilidadeRecorrente
        |-- gera HorarioMonitoria para as próximas semanas
        v
HorarioMonitoriaService.salvarEntidade
```

### `src/main/java/br/edu/udf/mione/security`

Camada transversal de autenticação e autorização:

- `SecurityConfig`: configura CORS, CSRF, sessão stateless, rotas públicas/autenticadas, filtro JWT, `PasswordEncoder`, `AuthenticationManager` e `PermissionEvaluator`.
- `JwtAuthenticationFilter`: lê JWT do cookie, valida token e preenche o contexto de segurança.
- `JwtUtil`: gera, valida e extrai claims do JWT.
- `CustomUserDetailService`: carrega usuário e perfis ativos para autenticação.
- `UsuarioSecurityDto`: fica em `dto`, mas é usado fortemente por esta camada.
- `CustomPermissionEvaluator`: suporte a autorização customizada.
- `SecurityAuthorityConfig`: configuração para authorities/roles.

Responsabilidade correta deste pacote: manter autenticação/autorização isolada da regra funcional, exceto pelos dados necessários de usuário/perfil.

### `src/main/java/br/edu/udf/mione/infra`

Infraestrutura transversal e utilitários de aplicação:

- `GlobalExceptionHandler`: tratamento global de exceções com `@ControllerAdvice`.
- `ApiResponse`: envelope padronizado de sucesso.
- `ApiErrorResponse`: envelope padronizado de erro.
- `ValidationException` e `IdNotFoundException`: exceções específicas da aplicação.
- `ConfiguracoesCors`: configuração adicional de CORS.
- `DisponibilidadeScheduler`: tarefa agendada que gera slots semanais de monitoria.
- `UserPasswordUpdater`: rotina de inicialização via `CommandLineRunner` para atualizar senha de usuário específico.

Responsabilidade correta deste pacote: componentes técnicos compartilhados e preocupações transversais que não pertencem a um caso de uso específico.

### `src/main/resources`

Recursos empacotados com a aplicação:

- `application.properties`: configura context path, porta, datasource, JPA, JWT, e-mail, validade de links e URL do frontend.
- `templates/`: templates HTML de e-mail, como agendamento, cancelamento, boas-vindas e troca de senha.
- `static/images/`: imagem estática usada nos templates, como `LogoUDF.png`.

### `src/test/java/br/edu/udf/mione`

Testes automatizados do backend:

- `IntegrationTestConfig`: configuração de integração/testes.
- `security/JwtAuthenticationFilterTest`: teste da camada de segurança.
- `service/*ServiceTest`: testes de services como atendimento, disponibilidade recorrente e horário de monitoria.

### `src/test/resources`

Recursos usados em testes:

- `application-test.yml`: propriedades específicas do ambiente de teste.
- `schema.sql`, `test-data.sql`, `test-data-config.sql`: estrutura e massa de dados para execução de testes.

## Dependências entre camadas

### Dependência ideal observada

```text
controller -> service -> repository -> model
dto        -> usado por controller, service e repository em projeções
security   -> intercepta request antes do controller e fornece principal autenticado
infra      -> trata erros, respostas, CORS e tarefas agendadas
```

### Pontos de acoplamento existentes

- Alguns controllers ainda acessam repository diretamente, como `AuthController` usando `UsuarioPerfilRepository` para listar/trocar perfil. Em uma arquitetura em camadas mais rígida, essa regra deveria estar totalmente em `UsuarioService`.
- Alguns controllers retornam entidades diretamente, por exemplo `Disciplina` em endpoints de coordenação. Para consistência de API, DTOs de response seriam mais estáveis.
- Alguns services usam `RuntimeException` genérica. O pacote `infra` já tem estrutura para padronizar exceções, mas o uso ainda não é uniforme.
- Há injeção por campo com `@Autowired` em alguns controllers, enquanto outros usam construtor. Injeção por construtor é mais consistente e testável.
- Foi encontrado um arquivo temporário `.SecurityConfig.java.swp` dentro de `security`; ele não faz parte da arquitetura e deveria ser removido ou ignorado pelo Git.

## Como ler a arquitetura do projeto

Para entender uma funcionalidade, comece pelo controller do perfil correspondente:

- Login e senha: `PublicAuthController` e `AuthController`.
- Aluno: `AlunoController`.
- Monitor: `MonitorController`.
- Coordenação/administração: `CoordenadorController` e `AdminController`.

Depois siga para o service injetado, sua implementação em `service/impl`, e por fim o repository usado. As entidades em `model` explicam como a informação está persistida.

Exemplo para agendamento de atendimento:

```text
AlunoController.criarAtendimento
        |
        v
AtendimentoService.agendar
        |
        v
AtendimentoServiceImpl
        |
        |-- valida aluno e horário
        |-- altera status do horário
        |-- cria Atendimento
        v
AtendimentoRepository / HorarioMonitoriaRepository
```


