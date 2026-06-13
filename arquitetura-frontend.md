# Arquitetura do Frontend

Este documento mapeia a arquitetura do frontend do projeto Mione, localizado em `frontend/`, com foco na estrutura de `src`, no fluxo entre camadas e na relação da implementação atual com o padrão Feature-Sliced Design (FSD).

## Resumo executivo

O frontend é uma aplicação React com Vite e TypeScript. A aplicação usa React Router para navegação, TanStack React Query para cache e sincronização de dados, Zustand para estado local persistido, Axios para HTTP, Tailwind CSS para estilos, Headless UI para componentes acessíveis e Lucide React para ícones.

O projeto foi estruturado seguindo uma adaptação pragmática de Feature-Sliced Design. As camadas principais do FSD aparecem claramente:

```text
src/
  app/
  pages/
  widgets/
  features/
  entities/
  shared/
```

O fluxo conceitual dominante é:

```text
main.tsx
  |
  v
app/providers
  |
  v
pages + widgets/layout
  |
  v
features
  |
  v
entities
  |
  v
shared/api, shared/ui, shared/lib
```

Em termos práticos, as telas ficam em `pages`, o layout principal em `widgets`, as ações de autenticação em `features/auth`, os conceitos de negócio em `entities`, e a infraestrutura reutilizável em `shared`.

## Enquadramento arquitetural

### Padrão principal: Feature-Sliced Design

Feature-Sliced Design é uma metodologia de arquitetura para frontends que organiza o código por camadas, slices e segmentos. A documentação oficial define camadas padronizadas como `app`, `pages`, `widgets`, `features`, `entities` e `shared`, e recomenda segmentos como `ui`, `api`, `model`, `lib` e `config`.

No projeto, a estrutura atual se encaixa nessa ideia:

- `app`: inicialização técnica da aplicação, providers, roteamento e estilos globais.
- `pages`: telas completas acessadas por rotas.
- `widgets`: blocos grandes e autocontidos de interface, como layout.
- `features`: funcionalidades acionáveis pelo usuário, como autenticação.
- `entities`: conceitos do domínio, como usuário, disciplina, perfil e serviço/monitoria.
- `shared`: código reutilizável, desacoplado do domínio específico.

### Regra de dependência esperada

No FSD, módulos de uma camada devem importar apenas de camadas abaixo. Aplicando isso ao projeto:

```text
app      -> pages, widgets, features, entities, shared
pages    -> widgets, features, entities, shared
widgets  -> features, entities, shared
features -> entities, shared
entities -> shared
shared   -> bibliotecas externas ou outros módulos de shared
```

O projeto segue essa direção na maior parte dos imports. Exemplos:

- `RouterProvider` em `app` importa `pages`, `widgets`, `entities` e `shared`.
- `MainLayout` em `widgets` importa `features/auth`, `entities/user` e `shared`.
- `LoginForm` em `features/auth` importa `entities/user`, `entities/profile` e `shared`.
- APIs de `entities` importam o client HTTP de `shared/api`.

### Aderência real ao FSD

O projeto está alinhado ao FSD na macroestrutura, mas não é uma implementação estrita em todos os detalhes.

Pontos alinhados:

- Camadas FSD principais existem e têm nomes esperados.
- `entities` usa slices de domínio com segmentos `api` e `model`.
- `features/auth` usa segmentos `api` e `ui`.
- `shared` centraliza client HTTP, UI kit, helpers, configurações e componentes genéricos.
- `app` concentra providers, roteamento e estilos globais.
- `widgets/layout` contém um bloco grande de interface reutilizado pela árvore protegida de rotas.

Adaptações e desvios:

- Muitos imports acessam arquivos internos diretamente, como `../../entities/user/model/store`, em vez de usar uma Public API por slice.
- Alguns componentes de `shared/ui` possuem `index.ts`, mas slices de `entities`, `features`, `pages` e `widgets` ainda não possuem Public API consistente.
- As páginas estão agrupadas por áreas (`auth`, `profile`, `management`, `workspaces`) e arquivos `.tsx` diretos, em vez de cada página ser uma slice com segmentos internos como `ui`, `api` e `model`.
- Parte da lógica de caso de uso ainda está dentro de páginas, por exemplo carregamento manual com `useEffect` em workspaces e páginas de gestão.
- `entities/service/api/serviceApi.ts` concentra operações de aluno e monitor; isso funciona, mas mistura casos de uso que poderiam virar features específicas se crescerem.

Esses pontos não impedem o projeto de ser entendido como FSD. Eles indicam uma adoção incremental e prática do padrão.

## Stack do frontend

Principais tecnologias identificadas:

- React `19.1.0`.
- Vite `6.3.5`.
- TypeScript `5.9.3`.
- React Router DOM `7.5.2`.
- TanStack React Query `5.80.5`.
- Axios `1.11.0`.
- Zustand `5.0.3`.
- Tailwind CSS `3.4.17`.
- Headless UI `2.2.7`.
- Lucide React `1.17.0`.
- React Hot Toast `2.6.0`.

O script de desenvolvimento sobe o Vite em `127.0.0.1:3000`. O frontend usa `/api` como base URL e o Vite faz proxy para o backend, por padrão em `http://127.0.0.1:8080`.

## Fluxo de inicialização

O ponto de entrada é `src/main.tsx`:

```text
main.tsx
  |
  |-- aplica tema salvo com shared/lib/theme
  |-- monta React.StrictMode
  |-- registra AppQueryProvider
  |-- registra AppRouterProvider
  |-- registra Toaster global
```

O roteamento fica em `src/app/providers/RouterProvider.tsx`. Ele cria as rotas públicas e protegidas:

- Públicas: login, cadastro público, cadastro por link, esqueci senha e reset de senha.
- Protegidas: dashboard, perfil, workspaces de aluno/monitor e gestão.

O `ProtectedRoute` usa:

- `useSession` de `entities/user/api/queries` para validar sessão via `/auth/me`.
- `useUserStore` de `entities/user/model/store` para checar perfil ativo.
- Redirecionamento para `/login` ou `/selecionar-perfil` quando necessário.

## Fluxo de dados

O fluxo HTTP principal é:

```text
Page ou Feature
  |
  v
entity api ou feature api
  |
  v
shared/api/baseApi.ts
  |
  v
Axios com baseURL /api e withCredentials
  |
  v
Backend Spring Boot
```

O JWT não fica no frontend. A autenticação depende de cookie `HttpOnly`, então o Axios está configurado com `withCredentials: true`.

O cache e sincronização de sessão usam React Query:

```text
useSession
  |
  v
userApi.me()
  |
  v
GET /api/auth/me
  |
  v
query cache ["users", "session"]
```

O Zustand guarda apenas estado complementar, como perfil ativo e lista de perfis. O store evita persistir usuário e token, respeitando a decisão de segurança do backend com cookie `HttpOnly`.

## Mapa dos diretórios de `frontend/src`

### `src/main.tsx`

Ponto de entrada da aplicação React. Responsável por montar a árvore principal, aplicar tema inicial, registrar providers globais e renderizar o `Toaster`.

Embora fique fora de `app`, ele funciona como entrypoint técnico da camada `app`.

### `src/app`

Camada mais alta da aplicação. Armazena configuração global, providers e estilos.

#### `src/app/index.css`

Estilos globais da aplicação, tokens visuais, Tailwind e variáveis de tema.

#### `src/app/providers`

Providers globais:

- `RouterProvider.tsx`: cria rotas, protege rotas autenticadas e conecta telas ao layout.
- `QueryProvider.tsx`: configura `QueryClientProvider` e defaults do React Query.

Responsabilidade correta: montar infraestrutura global da aplicação, não conter regra de negócio específica de uma tela.

### `src/pages`

Camada de telas completas. Cada arquivo representa uma rota ou tela de navegação.

#### `src/pages/auth`

Telas de autenticação e fluxos públicos:

- `LoginPage.tsx`: tela de login que usa `features/auth/ui/LoginForm`.
- `ForgotPasswordPage.tsx`: tela de solicitação de recuperação de senha.
- `ResetPasswordPage.tsx`: tela de redefinição de senha por token.
- `ProfileSelectionPage.tsx`: seleção/troca de perfil ativo.
- `PublicSignupPage.tsx`: cadastro público.
- `RegisterByLinkPage.tsx`: cadastro por link de aluno/monitor.

#### `src/pages/dashboard`

- `DashboardPage.tsx`: painel inicial autenticado. Renderiza conteúdo diferente conforme `activeProfile`.

#### `src/pages/error`

- `ErrorPage.tsx`: página de erro usada pelo React Router.

#### `src/pages/management`

Telas administrativas/de coordenação:

- `DisciplinasPage.tsx`: listagem, criação e edição de disciplinas.
- `UsuariosPage.tsx`: gestão de alunos, monitores, coordenadores/admins, ativação/desativação e links de cadastro.

#### `src/pages/profile`

Telas de perfil autenticado:

- `UserProfilePage.tsx`: visualização e edição dos dados do usuário logado.
- `ChangePasswordPage.tsx`: tela para alteração de senha.

#### `src/pages/workspaces`

Telas operacionais por perfil:

- `AlunoWorkspacePage.tsx`: agenda monitorias, lista horários disponíveis e gerencia atendimentos do aluno.
- `MonitorWorkspacePage.tsx`: gerencia horários, disponibilidades recorrentes e atendimentos do monitor.

Responsabilidade correta de `pages`: compor widgets, features, entities e shared para formar uma tela completa. Quando uma regra cresce ou é reutilizada, o ideal é extrair para `features` ou `entities`.

### `src/widgets`

Camada de blocos grandes de UI, geralmente reutilizáveis entre páginas ou responsáveis por uma parte estrutural da aplicação.

#### `src/widgets/layout`

- `MainLayout.tsx`: layout principal autenticado. Contém sidebar, header, menu de usuário, troca de tema, logout e `Outlet` para renderizar páginas filhas.

No FSD, esse diretório está bem classificado como widget: é um bloco autocontido que envolve várias páginas e entrega uma estrutura de uso completa.

### `src/features`

Camada de funcionalidades acionáveis pelo usuário. No projeto atual, há um slice principal:

#### `src/features/auth`

Funcionalidades de autenticação:

- `api/authApi.ts`: chamadas de login, logout, recuperação de senha, validação de token e cadastro por token.
- `ui/LoginForm.tsx`: formulário de login.
- `ui/ForgotPasswordForm.tsx`: formulário de recuperação de senha.
- `ui/ResetPasswordForm.tsx`: formulário de redefinição de senha.

Responsabilidade correta: guardar interações que geram valor direto para o usuário e podem ser reutilizadas em páginas diferentes. O slice `auth` está coerente com FSD.

### `src/entities`

Camada de entidades de negócio. Cada subdiretório representa um conceito do domínio ou uma área de dados usada pelo produto.

#### `src/entities/user`

Entidade de usuário e sessão:

- `api/userApi.ts`: chamadas para `/auth/me`, atualização de perfil e gestão de usuários.
- `api/queries.ts`: hooks React Query para sessão, listas, cadastro e ativação/desativação de perfis.
- `api/publicApi.ts`: chamadas públicas relacionadas a usuário, cadastro por link e recuperação.
- `model/store.ts`: store Zustand para perfil ativo, perfis disponíveis e estado complementar.
- `model/types.ts`: tipos de usuário.

#### `src/entities/profile`

Perfil de acesso:

- `api/profileApi.ts`: listagem e troca de perfis disponíveis.
- `model/types.ts`: tipos relacionados a perfil.

#### `src/entities/discipline`

Disciplina:

- `api/disciplineApi.ts`: chamadas de listagem, criação e atualização.
- `model/types.ts`: tipo `Discipline`.

#### `src/entities/service`

Operações de monitoria/atendimento:

- `api/serviceApi.ts`: chamadas para horários, atendimentos, disponibilidades recorrentes, agendamento e cancelamento.
- `model/types.ts`: tipos do domínio de serviço/monitoria.

Observação: o nome `service` aqui representa um domínio funcional do produto, não a camada "service" do backend. Se esse módulo crescer, nomes mais específicos como `monitoring`, `appointment` ou `schedule` poderiam deixar o domínio mais explícito.

### `src/shared`

Camada de código reutilizável e independente de domínio específico.

#### `src/shared/api`

- `baseApi.ts`: instância Axios com `baseURL: '/api'`, `withCredentials: true` e interceptors para log, 401, 403 e mensagens de erro.
- `index.ts`: public API parcial do segmento.

Responsabilidade: infraestrutura HTTP compartilhada por features e entities.

#### `src/shared/config`

- `routes.ts`: constantes/configuração de rotas compartilhadas.

Responsabilidade: configurações globais sem dependência direta de uma feature.

#### `src/shared/lib`

Bibliotecas internas pequenas:

- `theme.ts`: leitura, aplicação e persistência de tema.
- `withoutTransition.ts`: helper para aplicar mudanças visuais sem transição.
- `queryClient.ts`: utilidades relacionadas a React Query/toast.
- `index.ts`: reexports.

Responsabilidade: funções reutilizáveis e sem domínio específico.

#### `src/shared/ui`

UI kit e componentes compartilhados:

- `Alert`: alertas visuais.
- `AuthShell`: estrutura visual para telas de autenticação.
- `Button`: botão reutilizável.
- `Card`: card compartilhado.
- `ConfirmDialog`: modal de confirmação.
- `GlobalToast`: toast global customizado.
- `Input`: input reutilizável.
- `Loader` e `PageLoader`: indicadores de carregamento.
- `PageHeader`: cabeçalho de página.
- `PageWrapper`: wrapper visual de página.
- `RouteErrorBoundary`: boundary de erro para rotas.
- `ThemeToggle`: seletor de tema.
- `icons`: ícones da aplicação.

Responsabilidade: componentes reutilizáveis que não conhecem regra específica de aluno, monitor, disciplina ou atendimento.

## Mapa das rotas

Rotas públicas:

```text
/login
/cadastro
/selecionar-perfil
/auth/cadastro/:type/:token
/public/auth/cadastro/:type/:token
/esqueci-a-senha
/reset-senha/:token
/public/auth/trocar-senha/:token
```

Rotas protegidas sob `MainLayout`:

```text
/dashboard
/perfil
/perfil/alterar-senha
/workspaces/aluno
/workspaces/monitor
/management/disciplinas
/management/usuarios
/
```

O acesso protegido depende de sessão válida no backend e de perfil ativo no Zustand.

## Relação com o backend

O frontend conversa com o backend sempre via `/api`, que é proxied pelo Vite:

```text
Browser
  |
  v
Vite dev server / frontend container
  |
  v
/api proxy
  |
  v
Backend Spring Boot em :8080
```

O arquivo `src/shared/api/baseApi.ts` é o ponto central de integração. O frontend não manipula token JWT diretamente; apenas envia cookies com `withCredentials`.

## Pontos de melhoria arquitetural

1. Criar Public APIs por slice.

   Hoje existem `index.ts` em vários componentes de `shared/ui`, mas faltam public APIs em `entities/user`, `entities/discipline`, `entities/profile`, `entities/service`, `features/auth`, `widgets/layout` e grupos de `pages`. Isso ajudaria a evitar imports profundos e reduziria acoplamento à estrutura interna.

2. Preferir imports via alias.

   O projeto já tem alias `@/*` configurado no `tsconfig.json` e no Vite, mas a maioria dos imports usa caminhos relativos longos. Em FSD, imports absolutos entre slices tornam a direção arquitetural mais legível.

3. Extrair regras reutilizáveis de páginas para features.

   `AlunoWorkspacePage`, `MonitorWorkspacePage`, `DisciplinasPage` e `UsuariosPage` carregam bastante lógica local. Se esses fluxos crescerem, podem virar features como `schedule-monitoring-session`, `manage-discipline`, `approve-user` ou `manage-recurring-availability`.

4. Uniformizar React Query.

   `entities/user/api/queries.ts` já usa React Query bem. Outras áreas ainda usam `useEffect` + `useState` manual. Criar hooks de query/mutation para `discipline` e `service` melhoraria cache, loading, invalidation e consistência.

5. Avaliar o slice `entities/service`.

   O módulo funciona, mas concentra operações de aluno, monitor, horários, atendimentos e disponibilidade. Separar por entidades ou features pode facilitar a evolução se a área crescer.

6. Remover duplicidade de loaders.

   Existem `shared/ui/Loader/PageLoader.tsx` e `shared/ui/PageLoader/PageLoader.tsx`. Se ambos não forem necessários, consolidar reduz confusão.

## Como navegar no frontend

Para entender uma tela:

1. Comece em `src/app/providers/RouterProvider.tsx` para localizar a rota.
2. Abra o arquivo correspondente em `src/pages`.
3. Veja quais `features`, `entities` e componentes `shared` a página importa.
4. Para chamadas HTTP, siga até `entities/*/api` ou `features/*/api`.
5. Para estado global de usuário/perfil, veja `entities/user/model/store.ts` e `entities/user/api/queries.ts`.
6. Para layout e navegação autenticada, veja `widgets/layout/MainLayout.tsx`.

Exemplo de login:

```text
/login
  |
  v
pages/auth/LoginPage
  |
  v
features/auth/ui/LoginForm
  |
  |-- authApi.login()
  |-- profileApi.listarPerfis()
  |-- useUserStore.setProfiles()
  v
shared/api/baseApi -> /api/public/auth/login
```

Exemplo de workspace do aluno:

```text
/workspaces/aluno
  |
  v
pages/workspaces/AlunoWorkspacePage
  |
  |-- serviceApi.listarHorariosAluno()
  |-- serviceApi.listarAtendimentosAluno()
  |-- serviceApi.agendarAtendimento()
  |-- serviceApi.cancelarAtendimento()
  v
shared/api/baseApi -> /api/aluno/*
```

## Fontes pesquisadas

- Feature-Sliced Design, página inicial: https://feature-sliced.design/
- FSD Overview: https://feature-sliced.design/docs/get-started/overview
- FSD Layers: https://feature-sliced.design/docs/reference/layers
- FSD Slices and Segments: https://feature-sliced.design/docs/reference/slices-segments
- FSD Public API: https://feature-sliced.design/docs/reference/public-api
