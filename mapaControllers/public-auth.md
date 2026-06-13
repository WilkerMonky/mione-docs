# PublicAuthController

Base: `/api/public/auth`

Autenticacao: endpoints publicos.

## Endpoints

| Metodo | Rota | Entrada | Saida |
| --- | --- | --- | --- |
| `POST` | `/api/public/auth/login` | `AuthRequestDto` | `ApiResponse<Void>` e cookie `token` |
| `POST` | `/api/public/auth/esqueci-senha?email={email}` | Query param `email` | `String` |
| `POST` | `/api/public/auth/redefinir-senha/{token}` | Path param `token` e `SenhaDto` | `ApiResponse<Void>` |
| `POST` | `/api/public/auth/link/validar-token/troca-senha/{token}` | Path param `token` | `ApiResponse<Boolean>` |
| `POST` | `/api/public/auth/link/validar-token/cadastro/aluno/{token}` | Path param `token` | `ApiResponse<Boolean>` |
| `POST` | `/api/public/auth/link/validar-token/cadastro/monitor/{token}` | Path param `token` | `ApiResponse<Boolean>` |

## `POST /api/public/auth/login`

Entrada:

```json
{
  "email": "usuario@email.com",
  "senha": "senha"
}
```

Saida:

```json
{
  "status": 200,
  "message": "Login realizado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

Observacao: o token JWT e retornado em cookie HTTP chamado `token`.

## `POST /api/public/auth/esqueci-senha?email={email}`

Entrada:

```json
{
  "query": {
    "email": "usuario@email.com"
  }
}
```

Saida:

```text
Mensagem textual retornada pelo servico de recuperacao de senha.
```

Observacao: este endpoint retorna `String`, nao `ApiResponse`.

## `POST /api/public/auth/redefinir-senha/{token}`

Entrada:

```json
{
  "path": {
    "token": "token-recebido-por-email"
  },
  "body": {
    "senhaNova": "novaSenha",
    "senhaConfirmada": "novaSenha"
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Senha redefinida com sucesso.",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## `POST /api/public/auth/link/validar-token/troca-senha/{token}`

Entrada:

```json
{
  "path": {
    "token": "token-recebido-por-email"
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Token válido para troca de senha.",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": true
}
```

## `POST /api/public/auth/link/validar-token/cadastro/aluno/{token}`

Entrada:

```json
{
  "path": {
    "token": "token-de-cadastro"
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Token válido para cadastro de aluno.",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": true
}
```

## `POST /api/public/auth/link/validar-token/cadastro/monitor/{token}`

Entrada:

```json
{
  "path": {
    "token": "token-de-cadastro"
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Token válido para cadastro de monitor.",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": true
}
```
