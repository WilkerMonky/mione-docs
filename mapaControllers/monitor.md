# MonitorController

Base: `/api/monitor`

Autenticacao: requer autoridade `ACESSO_MONITOR`.

## Endpoints

| Metodo | Rota | Entrada | Saida |
| --- | --- | --- | --- |
| `POST` | `/api/monitor/horario-monitoria` | `HorarioRequestDto` | `ApiResponse<String>` |
| `POST` | `/api/monitor/horario-monitoria/{id}/bloquear` | Path param `id` | `ApiResponse<String>` |
| `POST` | `/api/monitor/horario-monitoria/{id}/liberar` | Path param `id` | `ApiResponse<String>` |
| `GET` | `/api/monitor/horario-monitoria/meus-horarios` | Sem corpo | `ApiResponse<List<HorarioMonitoriaResponseDto>>` |
| `GET` | `/api/monitor/atendimento` | Sem corpo | `ApiResponse<List<AtendimentoResponseDto>>` |
| `POST` | `/api/monitor/atendimento/cancelar?atendimentoId={id}` | Query param `atendimentoId` | `ApiResponse<String>` |
| `GET` | `/api/monitor/horario-monitoria` | Sem corpo | `ApiResponse<List<HorarioMonitoriaResponseDto>>` |
| `POST` | `/api/monitor/disponibilidade-recorrente` | `DisponibilidadeRecorrenteRequestDTO` | `ApiResponse<String>` |
| `GET` | `/api/monitor/disponibilidade-recorrente` | Sem corpo | `ApiResponse<List<DisponibilidadeRecorrenteResponseDTO>>` |
| `POST` | `/api/monitor/disponibilidade-recorrente/ativar/{id}` | Path param `id` | `ApiResponse<String>` |
| `POST` | `/api/monitor/disponibilidade-recorrente/desativar/{id}` | Path param `id` | `ApiResponse<String>` |

## Horarios de monitoria

### `POST /api/monitor/horario-monitoria`

Entrada:

```json
{
  "disciplinaId": 1,
  "ano": 2026,
  "semestre": 1,
  "inicio": "2026-06-20T14:00:00",
  "fim": "2026-06-20T15:00:00",
  "disponibilidadeRecorrenteId": null
}
```

Saida:

```json
{
  "status": 200,
  "message": "Horário registrado com  sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### `POST /api/monitor/horario-monitoria/{id}/bloquear`

Entrada:

```json
{
  "path": {
    "id": 1
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Horário bloqueado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### `POST /api/monitor/horario-monitoria/{id}/liberar`

Entrada:

```json
{
  "path": {
    "id": 1
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Horário liberado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### `GET /api/monitor/horario-monitoria/meus-horarios`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Disponibilidades encontradas com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": [
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
  ],
  "metadata": {
    "count": 1
  }
}
```

### `GET /api/monitor/horario-monitoria`

Entrada:

```json
null
```

Saida: lista de `HorarioMonitoriaResponseDto` no envelope `ApiResponse`.

## Atendimentos

### `GET /api/monitor/atendimento`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Atendimentos do monitor encontrados com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": [
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
  ],
  "metadata": {
    "count": 1
  }
}
```

### `POST /api/monitor/atendimento/cancelar?atendimentoId={id}`

Entrada:

```json
{
  "query": {
    "atendimentoId": 1
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Atendimento cancelado com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## Disponibilidade recorrente

### `POST /api/monitor/disponibilidade-recorrente`

Entrada:

```json
{
  "disciplinaId": 1,
  "diaSemanaId": 2,
  "horaInicio": "14:00:00",
  "horaFim": "15:00:00",
  "periodoLetivoId": 1,
  "ativo": true
}
```

Saida:

```json
{
  "status": 201,
  "message": "Disponibilidade recorrente registrada com  sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### `GET /api/monitor/disponibilidade-recorrente`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "Disponibilidades recorrentes recuperadas com sucesso",
  "timestamp": "2026-06-13T12:00:00Z",
  "data": [
    {
      "id": 1,
      "disciplinaNome": "Calculo II",
      "diaSemanaNome": "Segunda-feira",
      "diaSemanaId": 2,
      "horaInicio": "14:00:00",
      "horaFim": "15:00:00",
      "periodoLetivoNome": "2026/1",
      "ativo": true
    }
  ],
  "metadata": {
    "count": 1
  }
}
```

### `POST /api/monitor/disponibilidade-recorrente/ativar/{id}`

Entrada:

```json
{
  "path": {
    "id": 1
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Disponibilidade ativada com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

### `POST /api/monitor/disponibilidade-recorrente/desativar/{id}`

Entrada:

```json
{
  "path": {
    "id": 1
  }
}
```

Saida:

```json
{
  "status": 200,
  "message": "Disponibilidade desativada com sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```
