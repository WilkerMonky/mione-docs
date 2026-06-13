# AdminController

Base: `/api/admin`

Autenticacao: requer autoridade `ACESSO_ADMIN`.

## Endpoints

| Metodo | Rota | Entrada | Saida |
| --- | --- | --- | --- |
| `POST` | `/api/admin/alunos` | `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `GET` | `/api/admin/alunos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/admin/alunos/inativos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/admin/alunos/link-cadastro?validadeMinutos={minutos}` | Query param opcional `validadeMinutos` | `ApiResponse<String>` |
| `POST` | `/api/admin/alunos/{usuarioId}/ativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/admin/alunos/{usuarioId}/desativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/admin/monitores` | `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `GET` | `/api/admin/monitores` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/admin/monitores/inativos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/admin/monitores/link-cadastro?validadeMinutos={minutos}` | Query param opcional `validadeMinutos` | `ApiResponse<String>` |
| `POST` | `/api/admin/monitores/{usuarioId}/ativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/admin/monitores/{usuarioId}/desativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/admin/coordenadores` | `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `GET` | `/api/admin/coordenadores` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/admin/coordenadores/inativos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `POST` | `/api/admin/coordenadores/{usuarioId}/ativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/admin/coordenadores/{usuarioId}/desativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/admin/admins` | `UsuarioRequestDto` | `ApiResponse<UsuarioResponseDto>` |
| `GET` | `/api/admin/admins` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `GET` | `/api/admin/admins/inativos` | Sem corpo | `ApiResponse<List<UsuarioResponseDto>>` |
| `POST` | `/api/admin/admins/{usuarioId}/ativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `POST` | `/api/admin/admins/{usuarioId}/desativar` | Path param `usuarioId` | `ApiResponse<Void>` |
| `GET` | `/api/admin/disciplinas` | Sem corpo | `ApiResponse<List<Disciplina>>` |
| `POST` | `/api/admin/disciplinas` | `Disciplina` | `ApiResponse<Disciplina>` |
| `PUT` | `/api/admin/disciplinas/{id}` | Path param `id` e `Disciplina` | `ApiResponse<Disciplina>` |

## Usuarios por perfil

Os grupos `alunos`, `monitores`, `coordenadores` e `admins` usam o mesmo formato de entrada e saida para cadastro, listagem e ativacao/desativacao.

### Cadastro direto

Rotas:

```text
POST /api/admin/alunos
POST /api/admin/monitores
POST /api/admin/coordenadores
POST /api/admin/admins
```

Entrada:

```json
{
  "id": null,
  "nome": "Nome completo",
  "email": "usuario@email.com",
  "telefone": "(61) 99999-9999",
  "senha": "senhaInicial"
}
```

Saida:

```json
{
  "status": 201,
  "message": "Usuario cadastrado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "id": 1,
    "nome": "Nome completo",
    "email": "usuario@email.com",
    "telefone": "(61) 99999-9999"
  }
}
```

Observacao: a mensagem exata varia conforme o perfil criado, por exemplo `Aluno cadastrado com sucesso`, `Monitor cadastrado com sucesso`, `Coordenador cadastrado com sucesso` ou `Administrador cadastrado com sucesso`.

### Listagem de ativos

Rotas:

```text
GET /api/admin/alunos
GET /api/admin/monitores
GET /api/admin/coordenadores
GET /api/admin/admins
```

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Usuarios ativos encontrados com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": [
    {
      "id": 1,
      "nome": "Nome completo",
      "email": "usuario@email.com",
      "telefone": "(61) 99999-9999"
    }
  ],
  "metadata": {
    "count": 1
  }
}
```

### Listagem de inativos

Rotas:

```text
GET /api/admin/alunos/inativos
GET /api/admin/monitores/inativos
GET /api/admin/coordenadores/inativos
GET /api/admin/admins/inativos
```

Entrada:

```json
null
```

Saida: lista de `UsuarioResponseDto` no envelope `ApiResponse`.

### Ativar usuario

Rotas:

```text
POST /api/admin/alunos/{usuarioId}/ativar
POST /api/admin/monitores/{usuarioId}/ativar
POST /api/admin/coordenadores/{usuarioId}/ativar
POST /api/admin/admins/{usuarioId}/ativar
```

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
  "message": "Perfil de usuario ativado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### Desativar usuario

Rotas:

```text
POST /api/admin/alunos/{usuarioId}/desativar
POST /api/admin/monitores/{usuarioId}/desativar
POST /api/admin/coordenadores/{usuarioId}/desativar
POST /api/admin/admins/{usuarioId}/desativar
```

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
  "message": "Perfil de usuario desativado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## Links de cadastro

Disponiveis apenas para alunos e monitores.

Rotas:

```text
GET /api/admin/alunos/link-cadastro?validadeMinutos={minutos}
GET /api/admin/monitores/link-cadastro?validadeMinutos={minutos}
```

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

## Disciplinas

### `GET /api/admin/disciplinas`

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

### `POST /api/admin/disciplinas`

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

### `PUT /api/admin/disciplinas/{id}`

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
