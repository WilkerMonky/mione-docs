# Contexto IA - Projeto Mione

Esta pasta foi criada para entregar um contexto organizado do projeto Mione para outra IA ou para um novo agente de desenvolvimento.

O objetivo nao e duplicar todo o codigo fonte, mas fornecer um mapa confiavel para que outra IA saiba:

- o que o sistema faz;
- como backend, frontend, Docker e documentacao se conectam;
- onde encontrar cada parte importante;
- quais arquivos devem ser consultados antes de alterar o projeto;
- quais arquivos ou diretorios devem ser ignorados por seguranca ou volume.

## Ordem recomendada para outra IA

1. [instrucoes-para-outra-ia.md](instrucoes-para-outra-ia.md)
2. [contexto-geral.md](contexto-geral.md)
3. [backend.md](backend.md)
4. [frontend.md](frontend.md)
5. [docker-e-execucao.md](docker-e-execucao.md)
6. [documentacao-e-endpoints.md](documentacao-e-endpoints.md)
7. [inventario-arquivos.md](inventario-arquivos.md)

## Arquivos desta pasta

- [instrucoes-para-outra-ia.md](instrucoes-para-outra-ia.md)
  - prompt inicial e regras para uma IA consumir o projeto.

- [contexto-geral.md](contexto-geral.md)
  - resumo do produto, perfis, dominio, fluxos principais e stack.

- [backend.md](backend.md)
  - mapa sintetico do backend, arquitetura em camadas, pacotes, entidades e responsabilidades.

- [frontend.md](frontend.md)
  - mapa sintetico do frontend, Feature-Sliced Design, camadas, rotas e fluxo de dados.

- [docker-e-execucao.md](docker-e-execucao.md)
  - como a aplicacao roda com Docker Compose, backend isolado e frontend local.

- [documentacao-e-endpoints.md](documentacao-e-endpoints.md)
  - onde estao documentos formais, mapas de endpoints e leitura recomendada.

- [inventario-arquivos.md](inventario-arquivos.md)
  - inventario dos arquivos relevantes e dos diretorios que devem ser ignorados.

## Documentos relacionados fora desta pasta

- [../arquitetura-backend.md](../arquitetura-backend.md)
- [../arquitetura-frontend.md](../arquitetura-frontend.md)
- [../mapaControllers/README.md](../mapaControllers/README.md)
- [../README.md](../README.md)
- [../../backend/README.md](../../backend/README.md)
- [../../frontend/README.md](../../frontend/README.md)

## Observacao de seguranca

Nao inclua arquivos `.env`, credenciais, tokens, senhas reais, chaves JWT, diretorios `.git`, `node_modules`, `target`, `build` ou artefatos temporarios em um pacote enviado para outra IA.
