---
title: "API Endpoints — TILA (Verified)"
type: concept
tags: [api, endpoints, backend, rest]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# API Endpoints — TILA (Verificado no Código)

> Auditoria completa dos endpoints reais encontrados no código em 2026-05-07.

## Convenção de Respostas
Wrapper `GenericResult<T>`:
```json
{ "success": true, "message": "Operação realizada com sucesso !", "data": { ... } }
```
⚠️ `GlobalExceptionHandler` retorna `ErrorDetalhe` (record separado) ao invés de `GenericResult.error()` — **inconsistente**.

---

## Autenticação (`/auth`)

| Método | Path | Auth | Body/Params | Returns | Issues |
|---|---|---|---|---|---|
| POST | `/auth/registrar` | ❌ Público | `@Valid DadosCadastroMedico` (record) | `GenericResult<Boolean>` (201) | ⚠️ Sem verificação de email duplicado explícita (depende de DB unique) |
| POST | `/auth/login` | ❌ Público | `@Valid DadosAutenticacao` (record) | `GenericResult<UserDTO>` + Set-Cookie accessToken | 🔴 `medico.get()` sem verificação — NPE se médico não existe |
| GET | `/auth/me` | 🔒 Autenticado | — | `GenericResult<UserProfileDTO>` | 🔴 `medico.get()` sem verificação — NPE se médico não existe |

---

## Pacientes (`/paciente`)

| Método | Path | Auth | Body/Params | Returns | Issues |
|---|---|---|---|---|---|
| POST | `/paciente` | 🔒 MEDICO/PACIENTE | `@Valid PacienteRequestDTO` + `@AuthenticationPrincipal` | `GenericResult<PacienteResponseDTO>` (201) | ✅ Valida CPF duplicado |
| GET | `/paciente` | 🔒 MEDICO/PACIENTE | `@AuthenticationPrincipal` | `GenericResult<List<PacienteResponseDTO>>` | 🟡 Sem paginação; 🔴 Retorna `List<Exame>` entity no DTO |
| GET | `/paciente/{id}` | 🔒 MEDICO/PACIENTE | `@PathVariable Long id` + `@AuthenticationPrincipal` | `GenericResult<PacienteResponseDTO>` | 🔴 Retorna `List<Exame>` entity no DTO |

⚠️ **Sem PUT ou DELETE** — não é possível editar ou excluir pacientes.
⚠️ **Rota sem /api prefix** — usa `/paciente` diretamente ao invés de `/api/pacientes`.

---

## Logs de Auditoria (`/logs`)

| Método | Path | Auth | Body/Params | Returns | Issues |
|---|---|---|---|---|---|
| GET | `/logs` | 🔒 Autenticado (qualquer role) | — | `GenericResult<List<LogAuditoria>>` | 🔴 Retorna entity diretamente; 🟡 Sem paginação; 🟡 Não restringe por role |

⚠️ **Qualquer usuário autenticado pode ver TODOS os logs** — deveria ser restrito a ADMIN.

---

## Endpoints que NÃO EXISTEM mas são esperados

| Domínio | Endpoints Esperados | Status |
|---|---|---|
| Exames | CRUD /api/exames | ❌ Não implementado |
| Laudos | CRUD /api/laudos, POST /api/laudos/generate/{exameId} | ❌ Não implementado |
| Agenda/Consultas | CRUD /api/agenda | ❌ Não implementado (frontend usa mock) |
| Médicos | CRUD /api/medicos | ❌ Não implementado (criado apenas via /auth/registrar) |
| Usuários | CRUD /api/usuarios | ❌ Não implementado |

---

## Security Gaps nos Endpoints

### 🔴 CRITICAL
1. `medico.get()` sem `orElseThrow()` em `/auth/login` e `/auth/me` — NPE crash
2. `GET /logs` acessível por qualquer role — dados de auditoria devem ser restritos a ADMIN
3. `PacienteResponseDTO` retorna `List<Exame>` entity — serialização circular, exposição de dados internos

### 🟡 MEDIUM
4. Sem paginação em `GET /paciente` e `GET /logs` — carrega tudo da base
5. Sem PUT/DELETE em Paciente — impossível corrigir dados errados (LGPD direito de retificação)
6. Sem rate limiting em nenhum endpoint
7. Sem CORS em produção (apenas localhost:4200)
8. `GlobalExceptionHandler` não retorna `GenericResult` — formato de erro inconsistente

### 🔵 LOW
9. Rota `/paciente` sem prefix `/api` — inconsistente com REST best practices
10. Sem documentação OpenAPI/Swagger

## Referências
- [[wiki/concepts/backend-services]]
- [[wiki/entities/spring-boot-backend]]
- [[wiki/decisions/ADR-002-api-response-pattern]]
- [[context/security-lgpd]]

## Backlinks
- [[wiki/overview]]
- [[wiki/entities/angular-frontend]]
