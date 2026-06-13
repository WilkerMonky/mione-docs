# Documentacao e endpoints

## Indice geral

O indice principal da documentacao esta em:

- `doc/README.md`

Ele explica como os documentos foram divididos e qual ordem de leitura seguir.

## Arquitetura

- `doc/arquitetura-backend.md`
  - backend Spring Boot;
  - arquitetura em camadas;
  - responsabilidades dos diretorios de `backend/src`.

- `doc/arquitetura-frontend.md`
  - frontend React/Vite;
  - Feature-Sliced Design;
  - responsabilidades dos diretorios de `frontend/src`.

## Endpoints

Os endpoints estao em:

- `doc/mapaControllers/README.md`

Arquivos por controller:

- `doc/mapaControllers/auth.md`
- `doc/mapaControllers/public-auth.md`
- `doc/mapaControllers/public.md`
- `doc/mapaControllers/coordenador.md`
- `doc/mapaControllers/admin.md`
- `doc/mapaControllers/aluno.md`
- `doc/mapaControllers/monitor.md`

Todas as rotas consideram o prefixo:

```text
/api
```

## Documentos formais

Ficam em `doc/finalizado/`:

- `Documento de Visao.docx`
- `Documento de Glossario.docx`
- `Documento de Requisitos.docx`
- `Documento de Regras de Negocio.docx`
- `Especificacao de Caso de Uso.docx`
- `Especificacao Tecnica.docx`
- `Modelo Estrategia de Testes.docx`
- `Plano de Teste.docx`

Esses sao os documentos formais preferenciais.

## Documentos historicos

Ficam em:

- `doc/Documentacao IBM base/`

Sao modelos e arquivos `.doc` mantidos como historico/base. Quando houver equivalente em `finalizado/`, priorize `finalizado/`.

## Documentos auxiliares

- `doc/CARTA.md`
  - carta de continuidade e colaboradores.

- `doc/melhorias-e-funcionalidades..md`
  - backlog de melhorias futuras e pontos de evolucao.

## Quando atualizar documentacao

Atualize documentacao quando:

- criar, remover ou alterar endpoint;
- mudar DTO relevante;
- mudar fluxo de login/perfis;
- mudar arquitetura de pastas;
- mudar compose, porta, proxy ou variaveis;
- adicionar regra de negocio importante;
- implementar melhoria previamente documentada.
