---
title: "ADR-002: API Response Pattern — GenericResult"
type: decision
tags: [architecture, adr, api, response-pattern]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# ADR-002: API Response Pattern — GenericResult<T>

## Status
Partially Adopted

## Context
APIs REST precisam de um formato de resposta consistente para que o frontend possa tratar sucesso e erro de forma unificada. O TILA adotou o padrão `GenericResult<T>` como wrapper universal.

## Decision
Usar `GenericResult<T>` com a seguinte estrutura:

```java
public class GenericResult<T> {
    private final boolean success;
    private final String message;
    private final T data;

    public static <T> GenericResult<T> success(T data);
    public static <T> GenericResult<T> success(T data, String message);
    public static <T> GenericResult<T> error(String message);
}
```

### Frontend
O frontend define `GenericResult<T>` como interface TypeScript:
```typescript
export interface GenericResult<T> {
  success: boolean;
  message: string;
  data: T;
}
```

Todos os API services usam `Observable<GenericResult<T>>` e verificam `res.success` no `tap()`.

## Compliance Snapshot (2026-05-07)

### ✅ Endpoints Conformes
| Endpoint | Response Type |
|---|---|
| POST /auth/registrar | `GenericResult<Boolean>` |
| POST /auth/login | `GenericResult<UserDTO>` |
| GET /auth/me | `GenericResult<UserProfileDTO>` |
| POST /paciente | `GenericResult<PacienteResponseDTO>` |
| GET /paciente | `GenericResult<List<PacienteResponseDTO>>` |
| GET /paciente/{id} | `GenericResult<PacienteResponseDTO>` |
| GET /logs | `GenericResult<List<LogAuditoria>>` |

### ❌ NÃO Conformes
| Cenário | Response Atual | Deveria Ser |
|---|---|---|
| EntityNotFoundException (404) | `ErrorDetalhe { mensagem }` | `GenericResult.error(message)` com status 404 |
| ValidationException (400) | `ErrorDetalhe { mensagem }` | `GenericResult.error(message)` com status 400 |
| Outras exceptions | Stack trace default do Spring | `GenericResult.error(message)` com status 500 |

## Consequences

### Positivas
- Frontend tem um único padrão para processar qualquer resposta
- `success: false` permite diferenciar erro de negócio de erro HTTP
- Mensagem humanizada inclusa na resposta

### Negativas
- `GlobalExceptionHandler` **não usa** `GenericResult` — retorna record privado `ErrorDetalhe`
- Frontend precisa tratar dois formatos: `GenericResult` (sucesso) e `ErrorDetalhe` (erro)
- `GenericResult.success()` hardcoda "Operação realizada com sucesso !" — mensagem genérica

## Ação Recomendada
Reescrever `GlobalExceptionHandler` para retornar `GenericResult.error()` em todas as exceptions, eliminando o `ErrorDetalhe`.

## Backlinks
- [[wiki/concepts/api-endpoints]]
- [[wiki/concepts/backend-services]]
