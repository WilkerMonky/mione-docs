# AlunoController

Base: `/api/aluno`

Autenticacao: requer autoridade `ACESSO_ALUNO`.

## Endpoints

| Metodo | Rota | Entrada | Saida |
| --- | --- | --- | --- |
| `POST` | `/api/aluno/atendimento` | `AtendimentoRequestDTO` | `ApiResponse<String>` |
| `GET` | `/api/aluno/atendimento` | Sem corpo | `ApiResponse<List<AtendimentoResponseDto>>` |
| `POST` | `/api/aluno/atendimento/cancelar?atendimentoId={id}` | Query param `atendimentoId` | `ApiResponse<String>` |
| `GET` | `/api/aluno/horario-monitoria` | Sem corpo | `ApiResponse<List<HorarioMonitoriaResponseDto>>` |

## `POST /api/aluno/atendimento`

Entrada:

```json
{
  "observacao": "Observacao opcional",
  "horarioMonitoriaId": 1
}
```

Saida:

```json
{
  "status": 200,
  "message": "Horário agendado com  sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## `GET /api/aluno/atendimento`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "atendimentos encontrados com sucesso",
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

## `POST /api/aluno/atendimento/cancelar?atendimentoId={id}`

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
  "message": "Horário desmarcado com  sucesso",
  "timestamp": "2026-06-13T12:00:00Z"
}
```

## `GET /api/aluno/horario-monitoria`

Entrada:

```json
null
```

Saida:

```json
{
  "status": 200,
  "message": "horários disponíveis encontrados com sucesso",
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
