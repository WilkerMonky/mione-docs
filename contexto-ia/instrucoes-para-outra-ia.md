# Instrucoes para outra IA

Use este arquivo como prompt inicial para uma IA que vai analisar ou modificar o projeto.

## Prompt sugerido

Voce esta analisando o projeto Mione, uma plataforma de monitorias academicas. Antes de sugerir ou alterar codigo, leia os documentos em `doc/contexto-ia/`, especialmente `contexto-geral.md`, `backend.md`, `frontend.md`, `docker-e-execucao.md` e `documentacao-e-endpoints.md`.

O projeto possui:

- backend Java Spring Boot em `backend/`;
- frontend React/Vite/TypeScript em `frontend/`;
- documentacao em `doc/`;
- Docker Compose na raiz para subir banco, backend e frontend juntos;
- Docker Compose em `backend/` para subir apenas backend e banco.

Respeite a arquitetura existente:

- backend em arquitetura em camadas: `controller -> service -> repository -> model`;
- frontend baseado em Feature-Sliced Design: `app`, `pages`, `widgets`, `features`, `entities`, `shared`;
- API com prefixo global `/api`;
- JWT em cookie `HttpOnly`, sem token no `localStorage`;
- frontend usa `/api` e Vite proxy para falar com backend.

Antes de implementar:

1. Localize a funcionalidade no mapa de arquitetura.
2. Consulte o controller ou pagina correspondente.
3. Siga o fluxo ate service/repository no backend ou feature/entity/shared no frontend.
4. Confira os endpoints em `doc/mapaControllers/`.
5. Evite alterar arquivos fora do escopo solicitado.

## Regras para trabalhar no projeto

- Nao ler nem expor `.env`.
- Nao versionar credenciais, tokens ou senhas reais.
- Nao incluir `node_modules`, `target`, `build`, `.git` ou artefatos gerados no contexto.
- Preservar documentos existentes quando atualizar o indice.
- Preferir mudancas pequenas e alinhadas aos padroes atuais.
- No backend, manter regra de negocio em `service/impl`, nao em controller.
- No frontend, manter infraestrutura reutilizavel em `shared`, dominio em `entities`, acoes em `features` e telas em `pages`.

## Perguntas uteis antes de mudar codigo

- A alteracao e de backend, frontend, documentacao ou infraestrutura?
- Existe endpoint documentado em `doc/mapaControllers/`?
- A regra pertence a um service do backend?
- No frontend, a logica pertence a `page`, `feature`, `entity` ou `shared`?
- A mudanca exige atualizar documentos em `doc/`?
