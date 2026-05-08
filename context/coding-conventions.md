---
title: "Coding Conventions — TILA (Verificado)"
type: context
tags: [conventions, backend, frontend, patterns]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Coding Conventions — TILA

> Baseado em auditoria do código real em 2026-05-07. Documenta o que o código **realmente faz**, não o que deveria fazer.

---

## Backend — Convenções Reais

### Pacote Base
`tecnologi.tila.tila` — ⚠️ nome não usual (tila repetido)

### DTO Pattern
- ✅ **Records** para todos os DTOs: DadosAutenticacao, DadosCadastroMedico, UserDTO, UserProfileDTO, PacienteRequestDTO, PacienteResponseDTO, ExameRequestDTO, ExameResponseDTO, ConsultaDTO
- ✅ Bean Validation nos records: `@NotBlank`, `@NotNull`, `@Email`, `@CPF`, `@Past`
- ⚠️ `PacienteResponseDTO` inclui `List<Exame>` (entity JPA) ao invés de DTO — violação do padrão
- ⚠️ `DadosResponseLogin` existe mas é legado (não usado)

### Response Pattern
- ✅ `GenericResult<T>` usado em **todos** os controllers
- ❌ `GlobalExceptionHandler` retorna `ErrorDetalhe` (record privado) ao invés de `GenericResult.error()`
- **Compliance**: ~85% (todos os endpoints happy path usam GenericResult, mas error path não)

### Injection Style
- **Misto**:
  - ✅ Constructor injection: PacienteService, logAuditoriaService, logAuditoriaController, AutenticacaoController, TokenService
  - ❌ `@Autowired` field injection: SecurityConfigurations, SecurityFilter, AutenticacaoService, PacienteController

### Exception Handling
- ✅ `@RestControllerAdvice` (`GlobalExceptionHandler`)
- Trata: EntityNotFoundException (404), ValidationException (400)
- ❌ Não trata: RuntimeException, NullPointerException, AuthenticationException
- ❌ Formato de resposta inconsistente (ErrorDetalhe vs GenericResult)

### Transaction Management
- ✅ `@Transactional` em `PacienteService.cadastrar()`
- ✅ `@Transactional(readOnly=true)` em `PacienteService.buscarTodosPacientes()`
- ❌ Sem `@Transactional` em `bucasPorId()` e `buscarPorCpf()`
- ❌ Sem `@Transactional` no logAuditoriaService

### Naming
- ✅ PascalCase para entities (Usuario, Medico, Paciente, Exame, Laudo)
- ✅ camelCase para fields
- ❌ `logAuditoriaController` e `logAuditoriaService` — camelCase ao invés de PascalCase
- ❌ `StatuExame` — typo (falta 's')
- ❌ `athenticate` — typo no pacote (falta 'u')
- ❌ `bucasPorId` e `bucasTodosPacientes` — typos no nome dos métodos

### Lombok Usage
- Todas as entities usam `@Getter @Setter @NoArgsConstructor @AllArgsConstructor`
- ⚠️ Laudo e ConhecimentoMedico não usam `@NoArgsConstructor` do Lombok — construtores manuais
- `GenericResult` usa `@Getter` apenas

---

## Frontend — Convenções Reais

### Component Style
- ✅ **100% Standalone** — nenhum NgModule
- ✅ Todos usam `standalone: true` no decorator
- ✅ Imports explícitos no decorator

### State Management
- ✅ **AuthStore**: signal-based com `signal()`, `computed()`, `effect()`
- ✅ **MedicalStore**: signal-based
- ⚠️ **57% dos componentes** usam signals para estado local
- ❌ LoginComponent, CadastroComponent, CadastroPacienteComponent usam plain properties

### HTTP Pattern
- ✅ HttpClient com `provideHttpClient(withInterceptors([authInterceptor]))`
- ✅ Todos os API services retornam `Observable<GenericResult<T>>`
- ✅ Padrão `tap({ next: ... , error: ... })` em todos os services
- ✅ `firstValueFrom()` usado para converter Observable → Promise em componentes async
- ⚠️ `withCredentials: true` setado inconsistentemente: no interceptor E redundantemente em PacienteApiService

### CSS
- ✅ **Vanilla CSS** — sem frameworks CSS
- ✅ CSS custom properties (variáveis globais)
- ✅ Arquivos CSS separados (não inline)
- ✅ Inter (Google Fonts) como tipografia

### Routing
- ❌ **0% lazy loading** — todos os componentes importados eagerly
- ✅ authGuard em todas as rotas protegidas
- ✅ Wildcard route `**` → redirect login

### DI Style
- ✅ **100% `inject()` function** — nenhum construtor para DI
- ⚠️ SidebarComponent mistura `inject()` com `@Input/@Output` (aceitável)

### Forms
- ✅ Template-driven com FormsModule + `[(ngModel)]`
- ❌ Sem ReactiveFormsModule em nenhum componente
- Validação manual inline (sem Angular validators)

---

## Divergências — Convenção Esperada vs Encontrada

| Arquivo | Convenção | Esperado | Encontrado | Severidade |
|---|---|---|---|---|
| `PacienteController.java` | DI | Constructor injection | `@Autowired` field | 🟡 |
| `SecurityConfigurations.java` | DI | Constructor injection | `@Autowired` field | 🟡 |
| `SecurityFilter.java` | DI | Constructor injection | `@Autowired` field | 🟡 |
| `AutenticacaoService.java` | DI | Constructor injection | `@Autowired` field | 🟡 |
| `logAuditoriaController.java` | Naming | PascalCase | camelCase | 🟡 |
| `logAuditoriaService.java` | Naming | PascalCase | camelCase | 🟡 |
| `service/athenticate/` | Naming | `authenticate` | `athenticate` (typo) | 🟡 |
| `PacienteResponseDTO.java` | DTO pattern | DTO only | `List<Exame>` entity | 🔴 |
| `GlobalExceptionHandler.java` | Response pattern | `GenericResult.error()` | `ErrorDetalhe` | 🟡 |
| `AutenticacaoController.java` | Optional handling | `orElseThrow()` | `.get()` | 🔴 |
| `logAuditoriaService.java` | Empty result | Return empty list | throws RuntimeException | 🟡 |
| `app.routes.ts` | Lazy loading | `loadComponent: () => import(...)` | Direct import | 🟡 |
| LoginComponent | Signals | `signal()` para estado | plain properties | 🔵 |
| CadastroComponent | Signals | `signal()` para estado | plain properties | 🔵 |
| CadastroPacienteComponent | Signals | `signal()` para estado | plain properties | 🔵 |

## Referências
- [[wiki/concepts/backend-services]]
- [[wiki/concepts/frontend-architecture]]
- [[wiki/concepts/api-endpoints]]

## Backlinks
- [[wiki/overview]]
- [[context/roadmap]]
