# PublicController

Base: `/api/public`

Autenticacao: endpoints publicos.

## Endpoints

| Metodo | Rota | Entrada | Saida |
| --- | --- | --- | --- |
| `GET` | `/api/public` | Sem corpo | `String` |
| `POST` | `/api/public/auth/cadastro/aluno/{token}` | Path param `token` e `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `POST` | `/api/public/auth/cadastro/monitor/{token}` | Path param `token` e `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `GET` | `/api/public/disciplinas` | Sem corpo | `ApiResponse<List<Disciplina>>` |
| `GET` | `/api/public/periodo-letivo/ativo` | Sem corpo | `ApiResponse<PeriodoLetivo>` |

## `GET /api/public`

Entrada:

```json
null
```

Saida:

```text
Livros! E inteligência! Há coisas mais importantes: amizade e bravura.
```

## `POST /api/public/auth/cadastro/aluno/{token}`

Entrada:

```json
{
  "path": {
    "token": "token-de-cadastro"
  },
  "body": {
    "id": null,
    "nome": "Nome completo",
    "email": "aluno@email.com",
    "telefone": "(61) 99999-9999",
    "senha": "senhaInicial"
  }
}
```

Saida:

```json
{
  "status": 201,
  "message": "Aluno cadastrado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "id": 1,
    "nome": "Nome completo",
    "email": "aluno@email.com",
    "telefone": "(61) 99999-9999"
  }
}
```

## `POST /api/public/auth/cadastro/monitor/{token}`

Entrada:

```json
{
  "path": {
    "token": "token-de-cadastro"
  },
  "body": {
    "id": null,
    "nome": "Nome completo",
    "email": "monitor@email.com",
    "telefone": "(61) 99999-9999",
    "senha": "senhaInicial"
  }
}
```

Saida:

```json
{
  "status": 201,
  "message": "Monitor cadastrado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "id": 2,
    "nome": "Nome completo",
    "email": "monitor@email.com",
    "telefone": "(61) 99999-9999"
  }
}
```

## `GET /api/public/disciplinas`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Disciplinas encontradas com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": [
    {
      "disciplinaId": 1,
      "nome": "Calculo II",
      "codigo": "CALCUL"
    }
  ],
  "metadata": {
    "count": 1
  }
}
```

## `GET /api/public/periodo-letivo/ativo`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Período Letivo ativo encontrado",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "periodoLetivoId": 1,
    "ano": 2026,
    "semestre": 1,
    "dataInicio": "2026-02-01",
    "dataFim": "2026-07-01",
    "ativo": true,
    "descricao": "2026/1"
  }
}
```
