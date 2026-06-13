# Mapa de controllers

Este diretorio documenta os endpoints ativos do backend do projeto Mione. As rotas ja consideram o prefixo global configurado em `application.properties`:

```properties
server.servlet.context-path=/api
```

## Controllers mapeados

| Arquivo | Controller | Base |
| --- | --- | --- |
| [auth.md](auth.md) | `AuthController` | `/api/auth` |
| [public-auth.md](public-auth.md) | `PublicAuthController` | `/api/public/auth` |
| [public.md](public.md) | `PublicController` | `/api/public` |
| [coordenador.md](coordenador.md) | `CoordenadorController` | `/api/coordenador` |
| [admin.md](admin.md) | `AdminController` | `/api/admin` |
| [aluno.md](aluno.md) | `AlunoController` | `/api/aluno` |
| [monitor.md](monitor.md) | `MonitorController` | `/api/monitor` |

## Envelope de sucesso

A maior parte dos endpoints retorna `ApiResponse<T>`.

```json
{
  "status": 200,
  "message": "Mensagem da operacao",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": {},
  "metadata": {
    "count": 1
  },
  "links": {}
}
```

Campos opcionais podem ser omitidos pelo backend quando estiverem vazios ou nulos.

## Envelope de erro

Erros tratados retornam `ApiErrorResponse`.

```json
{
  "status": 400,
  "message": "Mensagem generica do erro",
  "errorCode": "CODIGO_DO_ERRO",
  "timestamp": "2026-06-13T12:00:00Z",
  "path": "/api/rota",
  "requestId": "abc-123",
  "details": [],
  "fieldErrors": {}
}
```

## DTOs comuns

### `UsuarioRequestDto`

```json
{
  "id": null,
  "nome": "Nome completo",
  "email": "usuario@email.com",
  "telefone": "(61) 99999-9999",
  "senha": "senhaInicial"
}
```

### `UsuarioResponseDto`

```json
{
  "id": 1,
  "nome": "Nome completo",
  "email": "usuario@email.com",
  "telefone": "(61) 99999-9999"
}
```

### `UsuarioSimplesDto`

```json
{
  "idUsuario": 1,
  "nome": "Nome completo",
  "email": "usuario@email.com",
  "telefone": "(61) 99999-9999"
}
```

### `Disciplina`

Entrada usada no cadastro e edicao:

```json
{
  "nome": "Calculo II"
}
```

Resposta:

```json
{
  "disciplinaId": 1,
  "nome": "Calculo II",
  "codigo": "CALCUL"
}
```

### `PeriodoLetivo`

```json
{
  "periodoLetivoId": 1,
  "ano": 2026,
  "semestre": 1,
  "dataInicio": "2026-02-01",
  "dataFim": "2026-07-01",
  "ativo": true,
  "descricao": "2026/1"
}
```

### `HorarioMonitoriaResponseDto`

```json
{
  "id": 1,
  "inicio": "2026-06-20T14:00:00",
  "fim": "2026-06-20T15:00:00",
  "statusNome": "DISPONIVEL",
  "disciplinaNome": "Calculo II",
  "monitor": {
    "idUsuario": 2,
    "nome": "Monitor",
    "email": "monitor@email.com",
    "telefone": "(61) 99999-9999"
  }
}
```

### `AtendimentoResponseDto`

```json
{
  "atendimentoId": 1,
  "observacao": "Observacao opcional",
  "criadoEm": "2026-06-13T10:00:00",
  "aluno": {
    "idUsuario": 3,
    "nome": "Aluno",
    "email": "aluno@email.com",
    "telefone": "(61) 99999-9999"
  },
  "horarioMonitoria": {
    "id": 1,
    "inicio": "2026-06-20T14:00:00",
    "fim": "2026-06-20T15:00:00",
    "statusNome": "AGENDADO",
    "disciplinaNome": "Calculo II",
    "monitor": {
      "idUsuario": 2,
      "nome": "Monitor",
      "email": "monitor@email.com",
      "telefone": "(61) 99999-9999"
    }
  },
  "status": "AGENDADO"
}
```

