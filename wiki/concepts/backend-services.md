---
title: "Backend Services — TILA"
type: concept
tags: [backend, services, repositories, architecture]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Backend Services — TILA

## Inventário de Services

| Service | Métodos | Dependencies (DI) | Issues |
|---|---|---|---|
| **PacienteService** | `cadastrar()`, `buscarTodosPacientes()`, `buscarPorCpf()`, `bucasPorId()`, `registrarLog()` | PacienteRepository, LogAuditoriaRepository (constructor) | ✅ Constructor injection; ⚠️ `bucasPorId` tem typo |
| **AutenticacaoService** | `loadUserByUsername()` | UsuarioRepository | 🔴 `@Autowired` field injection |
| **logAuditoriaService** | `buscarTodosOsLogs()` | LogAuditoriaRepository (constructor) | ⚠️ Nome violando PascalCase; ⚠️ throws RuntimeException para lista vazia |
| **TokenService** | `gerarToken()`, `getSubject()`, `dataExpiracao()` | `@Value` secret | ⚠️ Secret hardcoded no .properties |
| **GenericResult** | `success()` (×2), `error()` | Nenhum (POJO) | ✅ Bem implementado |

### Detalhamento dos Métodos

#### PacienteService
| Método | Params | Retorno | @Transactional | Propósito |
|---|---|---|---|---|
| `cadastrar` | PacienteRequestDTO, Usuario | PacienteResponseDTO | ✅ `@Transactional` | Cria paciente + log |
| `buscarTodosPacientes` | Usuario | List<PacienteResponseDTO> | ✅ `readOnly=true` | Lista todos (⚠️ sem paginação) |
| `buscarPorCpf` | String cpf, Usuario | PacienteResponseDTO | ❌ Sem @Transactional | Busca por CPF |
| `bucasPorId` | Long id, Usuario | PacienteResponseDTO | ❌ Sem @Transactional | Busca por ID (⚠️ typo no nome) |
| `registrarLog` | Usuario, String, LocalDateTime | void | ❌ Sem @Transactional | Helper para auditoria |

#### TokenService
| Método | Params | Retorno | Propósito |
|---|---|---|---|
| `gerarToken` | Usuario | String (JWT) | Gera token com email como subject, role como claim, 1h expiry |
| `getSubject` | String tokenJWT | String (email) | Valida token e extrai subject |

---

## Inventário de Repositories

| Repository | Entity | Extends | Custom Queries | Issues |
|---|---|---|---|---|
| **UsuarioRepository** | Usuario | JpaRepository<UUID> | `findByEmail(String)` | ✅ OK |
| **MedicoRepository** | Medico | JpaRepository<Long> | `findByCrm(String)`, `findByUsuario(Usuario)` | ✅ OK |
| **PacienteRepository** | Paciente | JpaRepository<Long> | `findByCpf(String)`, `findByNomeCompletoContainingIgnoreCase(String)`, `existsByCpf(String)` | ✅ OK |
| **LogAuditoriaRepository** | LogAuditoria | JpaRepository<Long> | `findByUsuarioIdOrderByDataHoraDesc(UUID)`, `findByDataHoraBetween(LocalDateTime, LocalDateTime)` | ✅ OK |
| **LaudoRepository** | Laudo | JpaRepository<Long> | `findByMedicoAndStatus(long, StatusLaudo)`, `findByExameId(long)` | ⚠️ Método usa `long` primitivo ao invés de `Long` wrapper — pode dar NPE |
| **ConhecimentoMedicoRepository** | ConhecimentoMedico | JpaRepository<Long> | Nenhuma | ✅ OK (simples) |
| **ConsultaRepository** | — | Nenhum | Nenhuma | 🔴 Interface vazia, nem extends JpaRepository |

### Riscos de N+1
- ⚠️ `PacienteService.buscarTodosPacientes()` usa `findAll()` e acessa `p.getExames()` no stream → **N+1 clássico** se exames são lazy
- ⚠️ `PacienteResponseDTO` inclui `List<Exame>` diretamente — se EAGER, carrega todos exames de todos pacientes
- ⚠️ Nenhum `@EntityGraph` definido em nenhum repository

---

## Violações de Convenção

| Arquivo | Convenção | Esperado | Encontrado | Severidade |
|---|---|---|---|---|
| `AutenticacaoService.java` | DI Pattern | Constructor injection | `@Autowired` field injection | 🟡 MEDIUM |
| `SecurityConfigurations.java` | DI Pattern | Constructor injection | `@Autowired` field injection | 🟡 MEDIUM |
| `SecurityFilter.java` | DI Pattern | Constructor injection | `@Autowired` field injection | 🟡 MEDIUM |
| `logAuditoriaService.java` | Java Naming | `LogAuditoriaService` | `logAuditoriaService` (camelCase) | 🟡 MEDIUM |
| `logAuditoriaController.java` | Java Naming | `LogAuditoriaController` | `logAuditoriaController` (camelCase) | 🟡 MEDIUM |
| `PacienteService.java` | Method Naming | `buscarPorId` | `bucasPorId` (typo) | 🔵 LOW |
| `PacienteController.java` | DI Pattern | Constructor injection | `@Autowired` field injection | 🟡 MEDIUM |
| `PacienteController.java` | Method Naming | `buscarTodosPacientes` | `bucasTodosPacientes` (typo) | 🔵 LOW |
| `service/athenticate/` | Package Naming | `authenticate` | `athenticate` (typo) | 🟡 MEDIUM |
| `logAuditoriaService.java` | Exception Handling | Retornar lista vazia ou GenericResult.error | throws RuntimeException | 🟡 MEDIUM |
| `GlobalExceptionHandler.java` | Response Pattern | `GenericResult` envelope | `ErrorDetalhe` record (inconsistente) | 🟡 MEDIUM |
| `PacienteResponseDTO.java` | DTO Pattern | Usar DTOs para relações | Retorna `List<Exame>` (entity) | 🔴 HIGH |
| `AutenticacaoController.java` | Safety | `orElseThrow()` | `medico.get()` — pode dar NoSuchElementException | 🔴 HIGH |

## Backlinks
- [[wiki/entities/spring-boot-backend]]
- [[wiki/concepts/data-model]]
- [[wiki/concepts/api-endpoints]]
- [[context/coding-conventions]]
