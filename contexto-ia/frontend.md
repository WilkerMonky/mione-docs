# Contexto do frontend

## Localizacao

O frontend fica em `frontend/`.

Arquivos principais:

- `frontend/package.json`
- `frontend/vite.config.ts`
- `frontend/src/main.tsx`
- `frontend/src/app/providers/RouterProvider.tsx`
- `frontend/src/app/providers/QueryProvider.tsx`
- `frontend/src/shared/api/baseApi.ts`
- `frontend/Dockerfile`
- `frontend/README.md`

## Arquitetura

O frontend foi baseado em Feature-Sliced Design (FSD), com camadas principais:

```text
src/
  app/
  pages/
  widgets/
  features/
  entities/
  shared/
```

O documento completo esta em `doc/arquitetura-frontend.md`.

## Fluxo de inicializacao

```text
src/main.tsx
  |
  |-- aplica tema salvo
  |-- registra AppQueryProvider
  |-- registra AppRouterProvider
  |-- registra Toaster
  v
React app
```

## Camadas

- `app`
  - providers, roteamento e estilos globais;
  - contem `RouterProvider.tsx`, `QueryProvider.tsx` e `index.css`.

- `pages`
  - telas completas ligadas a rotas;
  - exemplos: login, dashboard, perfil, gestao de usuarios, workspaces.

- `widgets`
  - blocos grandes e autocontidos;
  - principal exemplo: `widgets/layout/MainLayout.tsx`.

- `features`
  - funcionalidades acionaveis pelo usuario;
  - principal slice atual: `features/auth`.

- `entities`
  - entidades e dados de dominio;
  - slices: `user`, `profile`, `discipline`, `service`.

- `shared`
  - infraestrutura e UI reutilizavel;
  - inclui `api`, `config`, `lib`, `ui`.

## Rotas principais

Publicas:

- `/login`
- `/cadastro`
- `/selecionar-perfil`
- `/auth/cadastro/:type/:token`
- `/public/auth/cadastro/:type/:token`
- `/esqueci-a-senha`
- `/reset-senha/:token`
- `/public/auth/trocar-senha/:token`

Protegidas:

- `/dashboard`
- `/perfil`
- `/perfil/alterar-senha`
- `/workspaces/aluno`
- `/workspaces/monitor`
- `/management/disciplinas`
- `/management/usuarios`

## Dados e estado

- Axios centralizado em `shared/api/baseApi.ts`.
- Base URL: `/api`.
- `withCredentials: true`, porque o JWT fica em cookie `HttpOnly`.
- React Query usado para sessao e parte da gestao de usuarios.
- Zustand usado em `entities/user/model/store.ts` para perfil ativo e perfis disponiveis.

## Relacao com backend

O frontend chama `/api`, e o Vite faz proxy para o backend:

```text
Frontend -> /api -> Backend Spring Boot :8080
```

Config:

- local: fallback `http://127.0.0.1:8080`;
- Docker Compose: `VITE_API_PROXY_TARGET=http://app:8080`.

## Pontos de atencao

- O projeto tem macroestrutura FSD, mas ainda usa imports relativos profundos.
- Nem todos os slices possuem Public API (`index.ts`) propria.
- Algumas paginas concentram logica local com `useEffect` e `useState`.
- `entities/service` mistura operacoes de aluno, monitor, horarios, atendimentos e disponibilidade.
- Existe duplicidade potencial de loader entre `shared/ui/Loader` e `shared/ui/PageLoader`.
- Ao alterar arquitetura, atualizar `doc/arquitetura-frontend.md`.
