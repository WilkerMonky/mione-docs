# CoordenadorController

Base: `/api/coordenador`

Autenticacao: requer autoridade `ACESSO_COORDENADOR` ou `ACESSO_ADMIN`.

## Endpoints

| Metodo | Rota | Entrada | Saida |
| --- | --- | --- | --- |
| `POST` | `/api/coordenador/alunos` | `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `GET` | `/api/coordenador/alunos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/coordenador/alunos/inativos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/coordenador/alunos/link-cadastro?validadeMinutos={minutos}` | Query param opcional `validadeMinutos` | `ApiResponse<String>` |
| `POST` | `/api/coordenador/alunos/{usuarioId}/ativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/coordenador/alunos/{usuarioId}/desativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/coordenador/monitores` | `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `GET` | `/api/coordenador/monitores` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/coordenador/monitores/inativos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/coordenador/monitores/link-cadastro?validadeMinutos={minutos}` | Query param opcional `validadeMinutos` | `ApiResponse<String>` |
| `POST` | `/api/coordenador/monitores/{usuarioId}/ativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/coordenador/monitores/{usuarioId}/desativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `GET` | `/api/coordenador/disciplinas` | Sem corpo | `ApiResponse<List<Disciplina>>` |
| `POST` | `/api/coordenador/disciplinas` | `Disciplina` | `ApiResponse<Disciplina>` |
| `PUT` | `/api/coordenador/disciplinas/{id}` | Path param `id` e `Disciplina` | `ApiResponse<Disciplina>` |

## Alunos

### `POST /api/coordenador/alunos`

Entrada:

```json
{
  "id": null,
  "nome": "Nome completo",
  "email": "aluno@email.com",
  "telefone": "(61) 99999-9999",
  "senha": "senhaInicial"
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

### `GET /api/coordenador/alunos`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Alunos ativos encontrados com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": [
    {
      "id": 1,
      "nome": "Nome completo",
      "email": "aluno@email.com",
      "telefone": "(61) 99999-9999"
    }
  ],
  "metadata": {
    "count": 1
  }
}
```

### `GET /api/coordenador/alunos/inativos`

Entrada:

```json
null
```

Saida: mesmo formato de lista de `UsuarioResponseDto`.

### `GET /api/coordenador/alunos/link-cadastro?validadeMinutos={minutos}`

Entrada:

```json
{
  "query": {
    "validadeMinutos": 1440
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Link de cadastro de aluno gerado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": "http://localhost:5173/auth/cadastro/aluno/token"
}
```

Observacao: `validadeMinutos` e opcional. Quando omitido, o backend usa `1440`.

### `POST /api/coordenador/alunos/{usuarioId}/ativar`

Entrada:

```json
{
  "path": {
    "usuarioId": 1
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Perfil de aluno ativado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### `POST /api/coordenador/alunos/{usuarioId}/desativar`

Entrada:

```json
{
  "path": {
    "usuarioId": 1
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Perfil de aluno desativado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## Monitores

### `POST /api/coordenador/monitores`

Entrada:

```json
{
  "id": null,
  "nome": "Nome completo",
  "email": "monitor@email.com",
  "telefone": "(61) 99999-9999",
  "senha": "senhaInicial"
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

### `GET /api/coordenador/monitores`

Entrada:

```json
null
```

Saida: lista de `UsuarioResponseDto` no envelope `ApiResponse`.

### `GET /api/coordenador/monitores/inativos`

Entrada:

```json
null
```

Saida: lista de `UsuarioResponseDto` no envelope `ApiResponse`.

### `GET /api/coordenador/monitores/link-cadastro?validadeMinutos={minutos}`

Entrada:

```json
{
  "query": {
    "validadeMinutos": 1440
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Link de cadastro de monitor gerado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": "http://localhost:5173/auth/cadastro/monitor/token"
}
```

### `POST /api/coordenador/monitores/{usuarioId}/ativar`

Entrada:

```json
{
  "path": {
    "usuarioId": 2
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Perfil de monitor ativado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### `POST /api/coordenador/monitores/{usuarioId}/desativar`

Entrada:

```json
{
  "path": {
    "usuarioId": 2
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Perfil de monitor desativado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## Disciplinas

### `GET /api/coordenador/disciplinas`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Disciplinas listadas com sucesso",
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

### `POST /api/coordenador/disciplinas`

Entrada:

```json
{
  "nome": "Calculo II"
}
```

Saida:

```json
{
  "status": 201,
  "message": "Disciplina criada com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "disciplinaId": 1,
    "nome": "Calculo II",
    "codigo": "CALCUL"
  }
}
```

### `PUT /api/coordenador/disciplinas/{id}`

Entrada:

```json
{
  "path": {
    "id": 1
  },
  "body": {
    "nome": "Calculo II"
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Disciplina atualizada com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "disciplinaId": 1,
    "nome": "Calculo II",
    "codigo": "CALCUL"
  }
}
```
