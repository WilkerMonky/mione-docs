# AuthController

Base: `/api/auth`

Autenticacao: endpoints exigem usuario autenticado via cookie `token`, exceto quando indicado pelo proprio fluxo de logout.

## Endpoints

| Metodo | Rota | Entrada | Saida |
| --- | --- | --- | --- |
| `POST` | `/api/auth/logout` | Sem corpo | `ApiResponse<Void>` e remocao do cookie `token` |
| `POST` | `/api/auth/trocar-perfil` | `TrocaPerfilRequestDto` | `ApiResponse<Void>` e novo cookie `token` |
| `GET` | `/api/auth/perfis-disponiveis` | Sem corpo | `ApiResponse<List<PerfilDisponivel>>` |
| `GET` | `/api/auth/me` | Sem corpo | `ApiResponse<CurrentUserDto>` |
| `PUT` | `/api/auth/me` | `AtualizarMeuPerfilRequestDto` | `ApiResponse<CurrentUserDto>` e novo cookie `token` |

## `POST /api/auth/logout`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Logout realizado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## `POST /api/auth/trocar-perfil`

Entrada:

```json
{
  "perfil": "ACESSO_ALUNO"
}
```

Saida:

```json
{
  "status": 200,
  "message": "Perfil alterado para ACESSO_ALUNO",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

Observacao: o endpoint tambem atualiza o cookie `token` com o perfil ativo selecionado.

## `GET /api/auth/perfis-disponiveis`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Perfis encontrados",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": [
    {
      "perfil": "ACESSO_ALUNO",
      "descricao": "Aluno"
    }
  ],
  "metadata": {
    "count": 1
  }
}
```

## `GET /api/auth/me`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Dados do usuário recuperados com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "email": "usuario@email.com",
    "nome": "Nome completo",
    "telefone": "(61) 99999-9999",
    "perfilAtivo": "ACESSO_ALUNO"
  }
}
```

## `PUT /api/auth/me`

Entrada:

```json
{
  "nome": "Nome completo",
  "email": "usuario@email.com",
  "telefone": "(61) 99999-9999"
}
```

Saida:

```json
{
  "status": 200,
  "message": "Perfil atualizado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {
    "email": "usuario@email.com",
    "nome": "Nome completo",
    "telefone": "(61) 99999-9999",
    "perfilAtivo": "ACESSO_ALUNO"
  }
}
```

Observacao: o endpoint tambem atualiza o cookie `token` para refletir dados atualizados do usuario.
