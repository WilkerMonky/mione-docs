# Documentacao do Projeto

Este diretorio concentra a documentacao funcional, tecnica, arquitetural, de endpoints e de testes do projeto `Mione`.

O objetivo desta pasta e servir como ponto de entrada para entender:

- a visao e o escopo do produto;
- os termos do dominio;
- os requisitos e regras de negocio;
- os casos de uso;
- a arquitetura do backend e do frontend;
- os endpoints expostos pelo backend;
- a estrategia e o plano de testes;
- os materiais historicos e documentos de apoio.

## Estrutura geral

```text
doc/
  README.md
  CARTA.md
  arquitetura-backend.md
  arquitetura-frontend.md
  melhorias-e-funcionalidades..md
  contexto-ia/
  finalizado/
  mapaControllers/
  Documentacao IBM base/
```

## Documentos principais

### Arquitetura e mapas tecnicos

- [contexto-ia/README.md](contexto-ia/README.md)
  - pacote de contexto preparado para outra IA ou novo agente de desenvolvimento;
  - resume produto, backend, frontend, Docker, documentacao, endpoints e inventario de arquivos;
  - orienta quais arquivos devem ser consultados e quais devem ser ignorados por seguranca ou volume.

- [arquitetura-backend.md](arquitetura-backend.md)
  - mapeia a arquitetura do backend Spring Boot;
  - explica a organizacao de `backend/src`;
  - classifica o backend como arquitetura em camadas com Service Layer, Repository/Data Access Layer e DTO;
  - descreve responsabilidades de controllers, services, repositories, models, DTOs, security e infra.

- [arquitetura-frontend.md](arquitetura-frontend.md)
  - mapeia a arquitetura do frontend React/Vite;
  - explica a organizacao de `frontend/src`;
  - relaciona a estrutura com Feature-Sliced Design;
  - descreve as camadas `app`, `pages`, `widgets`, `features`, `entities` e `shared`;
  - aponta aderencias e adaptacoes praticas do FSD no projeto.

- [mapaControllers/README.md](mapaControllers/README.md)
  - indice dos endpoints ativos do backend;
  - documenta o prefixo global `/api`;
  - explica envelopes de sucesso e erro;
  - lista DTOs comuns usados nas rotas.

### Endpoints por controller

Os endpoints foram divididos por controller dentro de `mapaControllers/`:

- [mapaControllers/auth.md](mapaControllers/auth.md)
  - endpoints autenticados gerais, como logout, troca de perfil e usuario logado.

- [mapaControllers/public-auth.md](mapaControllers/public-auth.md)
  - endpoints publicos de autenticacao, login, recuperacao de senha e validacao de tokens.

- [mapaControllers/public.md](mapaControllers/public.md)
  - endpoints publicos auxiliares, como cadastro por link e consultas publicas.

- [mapaControllers/coordenador.md](mapaControllers/coordenador.md)
  - endpoints de coordenacao para alunos, monitores, disciplinas e links de cadastro.

- [mapaControllers/admin.md](mapaControllers/admin.md)
  - endpoints administrativos para coordenadores e administradores.

- [mapaControllers/aluno.md](mapaControllers/aluno.md)
  - endpoints do perfil aluno, como horarios disponiveis, agendamentos e cancelamentos.

- [mapaControllers/monitor.md](mapaControllers/monitor.md)
  - endpoints do perfil monitor, como horarios, disponibilidades recorrentes e atendimentos.

### Documentos finalizados

O diretorio [finalizado/](finalizado/) concentra os artefatos consolidados em `.docx`. Eles devem ser tratados como a versao principal dos documentos formais do projeto:

- [finalizado/Documento de Visão.docx](finalizado/Documento%20de%20Vis%C3%A3o.docx)
  - apresenta contexto, objetivo do produto, envolvidos, necessidades e visao geral.

- [finalizado/Documento de Glossário.docx](finalizado/Documento%20de%20Gloss%C3%A1rio.docx)
  - padroniza termos usados no projeto e no dominio de monitorias.

- [finalizado/Documento de Requisitos.docx](finalizado/Documento%20de%20Requisitos.docx)
  - consolida requisitos funcionais e nao funcionais.

- [finalizado/Documento de Regras de Negócio.docx](finalizado/Documento%20de%20Regras%20de%20Neg%C3%B3cio.docx)
  - descreve restricoes, politicas e comportamentos obrigatorios do sistema.

- [finalizado/Especificação de Caso de Uso.docx](finalizado/Especifica%C3%A7%C3%A3o%20de%20Caso%20de%20Uso.docx)
  - detalha atores, pre-condicoes, fluxos, excecoes e pos-condicoes.

- [finalizado/Especificação Técnica.docx](finalizado/Especifica%C3%A7%C3%A3o%20T%C3%A9cnica.docx)
  - descreve a realizacao tecnica, componentes, entidades, integracoes e arquitetura.

- [finalizado/Modelo Estratégia de Testes.docx](finalizado/Modelo%20Estrat%C3%A9gia%20de%20Testes.docx)
  - define abordagem geral, tipos de teste, recursos, pre-requisitos e cobertura.

- [finalizado/Plano de Teste.docx](finalizado/Plano%20de%20Teste.docx)
  - organiza escopo, execucao, matriz de requisitos de teste e entregaveis.

### Documentos auxiliares

- [CARTA.md](CARTA.md)
  - carta de continuidade do projeto;
  - apresenta contexto geral, status, orientacoes de continuidade e colaboradores.

- [melhorias-e-funcionalidades..md](melhorias-e-funcionalidades..md)
  - lista melhorias e funcionalidades futuras mapeadas para proximas versoes;
  - inclui pontos de evolucao de produto, qualidade, relatorios, interface, auditoria e testes.

### Documentos historicos/base

O diretorio `Documentacao IBM base/` armazena modelos e arquivos `.doc` usados como origem historica ou base para os documentos finalizados:

- `Ata de Reunião.doc`
- `Documento de Glossário.doc`
- `Documento de Protótipo.doc`
- `Documento de Regras de Negócio.doc`
- `Documento de Requisitos.doc`
- `Documento de Visão.doc`
- `Especificação Técnica.doc`
- `Especificação de Caso de Uso.doc`
- `Modelo Estratégia de Testes.doc`
- `Plano de Teste.doc`

Quando existir uma versao equivalente em `finalizado/`, a versao de `finalizado/` deve ser priorizada para leitura e manutencao.

## Como os documentos foram divididos

### Por finalidade

- Documentacao formal do produto: `finalizado/*.docx`.
- Arquitetura tecnica: `arquitetura-backend.md` e `arquitetura-frontend.md`.
- Contrato HTTP/API: `mapaControllers/*.md`.
- Contexto consolidado para IA: `contexto-ia/*.md`.
- Continuidade e contexto do projeto: `CARTA.md`.
- Evolucao futura: `melhorias-e-funcionalidades..md`.
- Modelos e historico: `Documentacao IBM base/`.

### Por area tecnica

- Backend:
  - [arquitetura-backend.md](arquitetura-backend.md)
  - [mapaControllers/](mapaControllers/)
  - [contexto-ia/backend.md](contexto-ia/backend.md)

- Frontend:
  - [arquitetura-frontend.md](arquitetura-frontend.md)
  - [contexto-ia/frontend.md](contexto-ia/frontend.md)

- Produto, negocio e testes:
  - [finalizado/](finalizado/)
  - [contexto-ia/contexto-geral.md](contexto-ia/contexto-geral.md)

- Execucao e infraestrutura:
  - [contexto-ia/docker-e-execucao.md](contexto-ia/docker-e-execucao.md)

## Ordem recomendada de leitura

Para entender o projeto de ponta a ponta:

1. [CARTA.md](CARTA.md)
2. [finalizado/Documento de Visão.docx](finalizado/Documento%20de%20Vis%C3%A3o.docx)
3. [finalizado/Documento de Glossário.docx](finalizado/Documento%20de%20Gloss%C3%A1rio.docx)
4. [finalizado/Documento de Requisitos.docx](finalizado/Documento%20de%20Requisitos.docx)
5. [finalizado/Documento de Regras de Negócio.docx](finalizado/Documento%20de%20Regras%20de%20Neg%C3%B3cio.docx)
6. [finalizado/Especificação de Caso de Uso.docx](finalizado/Especifica%C3%A7%C3%A3o%20de%20Caso%20de%20Uso.docx)
7. [arquitetura-backend.md](arquitetura-backend.md)
8. [arquitetura-frontend.md](arquitetura-frontend.md)
9. [mapaControllers/README.md](mapaControllers/README.md)
10. [finalizado/Especificação Técnica.docx](finalizado/Especifica%C3%A7%C3%A3o%20T%C3%A9cnica.docx)
11. [finalizado/Modelo Estratégia de Testes.docx](finalizado/Modelo%20Estrat%C3%A9gia%20de%20Testes.docx)
12. [finalizado/Plano de Teste.docx](finalizado/Plano%20de%20Teste.docx)
13. [melhorias-e-funcionalidades..md](melhorias-e-funcionalidades..md)

Para desenvolvedores que querem mexer direto no codigo:

1. [contexto-ia/README.md](contexto-ia/README.md)
2. [contexto-ia/instrucoes-para-outra-ia.md](contexto-ia/instrucoes-para-outra-ia.md)
3. [arquitetura-backend.md](arquitetura-backend.md)
4. [arquitetura-frontend.md](arquitetura-frontend.md)
5. [mapaControllers/README.md](mapaControllers/README.md)
6. Documento especifico do controller em `mapaControllers/`
7. [melhorias-e-funcionalidades..md](melhorias-e-funcionalidades..md)

Para uma IA externa consumir o projeto:

1. [contexto-ia/instrucoes-para-outra-ia.md](contexto-ia/instrucoes-para-outra-ia.md)
2. [contexto-ia/contexto-geral.md](contexto-ia/contexto-geral.md)
3. [contexto-ia/backend.md](contexto-ia/backend.md)
4. [contexto-ia/frontend.md](contexto-ia/frontend.md)
5. [contexto-ia/docker-e-execucao.md](contexto-ia/docker-e-execucao.md)
6. [contexto-ia/documentacao-e-endpoints.md](contexto-ia/documentacao-e-endpoints.md)
7. [contexto-ia/inventario-arquivos.md](contexto-ia/inventario-arquivos.md)

## Relacao entre os documentos

- A `Carta` apresenta o projeto e orienta continuidade.
- A `Visao` define problema, contexto e envolvidos.
- O `Glossario` define a linguagem comum.
- Os `Requisitos` definem o que o sistema precisa entregar.
- As `Regras de Negocio` definem restricoes e comportamentos obrigatorios.
- A `Especificacao de Caso de Uso` detalha fluxos funcionais.
- As arquiteturas de backend e frontend explicam como o sistema foi implementado.
- O `mapaControllers` descreve os contratos HTTP usados pelo frontend.
- O `contexto-ia` consolida o conhecimento minimo para outra IA navegar pelo projeto.
- A `Especificacao Tecnica` consolida a realizacao tecnica formal.
- A `Estrategia de Testes` define como a validacao deve ser conduzida.
- O `Plano de Teste` organiza a execucao dessa validacao.
- O arquivo de melhorias registra evolucoes planejadas ou recomendadas.

## Padrao adotado

Os documentos preenchidos seguem este padrao:

- nome do projeto: `Mione`;
- empresa/equipe: `Labtech - UDF`;
- backend: Java, Spring Boot, Spring Data JPA, Spring Security, PostgreSQL;
- frontend: React, Vite, TypeScript, Feature-Sliced Design;
- API: rotas documentadas considerando o prefixo global `/api`.

Sempre que um documento novo for preenchido ou revisado, esse padrao deve ser mantido para evitar inconsistencias.

## Manutencao

Ao atualizar a documentacao:

1. preserve a estrutura e a finalidade de cada diretorio;
2. atualize este `README.md` quando criar, mover ou remover documentos;
3. priorize arquivos em `finalizado/` quando houver duplicidade com `Documentacao IBM base/`;
4. mantenha `arquitetura-backend.md` alinhado ao codigo em `backend/src`;
5. mantenha `arquitetura-frontend.md` alinhado ao codigo em `frontend/src`;
6. atualize `mapaControllers/` sempre que endpoints, DTOs ou envelopes de resposta mudarem;
7. atualize `contexto-ia/` quando houver mudancas relevantes em arquitetura, execucao ou estrutura;
8. registre melhorias futuras em `melhorias-e-funcionalidades..md`;
9. evite criar copias paralelas desnecessarias dos mesmos documentos.

## Observacoes importantes

- Os arquivos `.docx` em `finalizado/` sao os artefatos formais principais.
- Os arquivos `.doc` em `Documentacao IBM base/` foram mantidos como origem historica ou modelo.
- Os arquivos `.md` sao preferenciais para mapas tecnicos, arquitetura, endpoints e documentos vivos de manutencao.
- A pasta `contexto-ia/` deve permanecer livre de segredos, `.env`, artefatos gerados e dependencias instaladas.
- O backend e o frontend tambem possuem seus proprios `README.md` nos respectivos diretorios de codigo.
