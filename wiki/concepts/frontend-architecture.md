---
title: "Frontend Architecture — TILA (Verificado)"
type: concept
tags: [frontend, angular, architecture, components]
sources: []
last_updated: 2026-05-07
---

# Frontend Architecture — TILA

> Auditoria completa baseada no código real em 2026-05-07.

## Stack Verificado
- **Angular**: 19.2.x
- **TypeScript**: 5.7.2
- **RxJS**: 7.8.x
- **Zone.js**: 0.15.x
- **Package Manager**: npm (⚠️ SKILL.md dizia Bun — não confirmado)
- **CSS**: Vanilla CSS (sem frameworks)
- **Build Tool**: @angular-devkit/build-angular 19.2.23

### Dependência Suspeita
- `"s": "^1.0.0"` em package.json — pacote desconhecido/acidental

---

## Mapa de Rotas

| Path | Component | Guard | Lazy Loading | Status |
|---|---|---|---|---|
| `/` | redirect → `/login` | — | — | ✅ |
| `/login` | LoginComponent | ❌ | ❌ | ✅ Funcional |
| `/cadastro` | CadastroComponent | ❌ | ❌ | ✅ Funcional |
| `/dashboard` | DashboardComponent | ✅ authGuard | ❌ | ✅ Funcional (dados mock) |
| `/pacientes` | PacientesComponent | ✅ authGuard | ❌ | ✅ Funcional |
| `/pacientes/novo` | CadastroPacienteComponent | ✅ authGuard | ❌ | ✅ Funcional |
| `/pacientes/:id` | ProntuarioComponent | ✅ authGuard | ❌ | ✅ Funcional |
| `/laudo/:id` | LaudoIaComponent | ✅ authGuard | ❌ | ⚠️ Mockado |
| `/laudos` | CentroLaudosComponent | ✅ authGuard | ❌ | ⚠️ Mockado |
| `/logs` | LogsComponent | ✅ authGuard | ❌ | ✅ Funcional |
| `/agenda` | AgendaComponent | ✅ authGuard | ❌ | ⚠️ Mock (sem backend) |
| `**` | redirect → `/login` | — | — | ✅ |

⚠️ **Lazy loading: 0% de adoção** — todos os componentes são importados eagerly no `app.routes.ts`.

---

## Inventário de Componentes

### Componentes de Página (10)

| Component | Standalone | Signals | DI Style | Status |
|---|---|---|---|---|
| LoginComponent | ✅ | ❌ (plain properties) | `inject()` | ✅ Funcional |
| CadastroComponent | ✅ | ❌ (plain properties) | `inject()` | ✅ Funcional |
| DashboardComponent | ✅ | ✅ (`signal`, `computed`) | `inject()` | ⚠️ Dados mock |
| PacientesComponent | ✅ | ✅ (`signal`, `computed`) | `inject()` | ✅ Funcional |
| CadastroPacienteComponent | ✅ | ❌ (plain properties) | `inject()` | ✅ Funcional |
| ProntuarioComponent | ✅ | ✅ (`signal`) | `inject()` | ✅ Funcional |
| LaudoIaComponent | ✅ | ✅ (`signal`) | `inject()` | ⚠️ Mockado |
| CentroLaudosComponent | ✅ | ✅ (`signal`, `computed`) | `inject()` | ⚠️ Mockado |
| LogsComponent | ✅ | ✅ (`signal`) | `inject()` | ✅ Funcional |
| AgendaComponent | ✅ | ✅ (`signal`) | `inject()` | ⚠️ Mock (sem backend) |

### Componentes Compartilhados (4)

| Component | Standalone | Signals | Props Pattern |
|---|---|---|---|
| HeaderComponent | ✅ | ❌ | — |
| FooterComponent | ✅ | ❌ | — |
| SidebarComponent | ✅ | ❌ (computed from store) | `@Input`/`@Output` |
| SecurityBadgesComponent | ✅ | ❌ | — |

### Conformidade de Componentes
- **Standalone**: ✅ 100% (14/14)
- **Signals adoption**: ⚠️ 57% (8/14 usam signals, 6 usam plain properties)
- **DI style**: ✅ 100% `inject()` function (exceto SidebarComponent que usa `inject()` + `@Input/@Output`)
- **CSS separado**: ✅ 100% (todos usam arquivo .css separado)

---

## Inventário de Services

| Service | Injectado em | Propósito |
|---|---|---|
| AuthApiService | LoginComponent, CadastroComponent | Login, register, getMe |
| PacienteApiService | PacientesComponent, CadastroPacienteComponent, ProntuarioComponent | CRUD pacientes |
| AgendaApiService | AgendaComponent | Appointments, waiting room, stats (sem backend) |
| LogAuditoriaService | LogsComponent | Buscar logs de auditoria |
| AuthStore | DashboardComponent, PacientesComponent, CadastroPacienteComponent, ProntuarioComponent, etc. | Estado de autenticação global |
| MedicalStore | (disponível, pouco usado) | Contexto médico (CPF selecionado, rascunho) |

---

## Patterns Observados

### AuthStore — Signal-Based State Management
- Usa `signal<AuthState>` + `computed` selectors + `effect()` para persistência em localStorage
- `fetchProfile()` usa `firstValueFrom()` para converter Observable → Promise
- ✅ Pattern bem implementado e consistente

### Interceptor
- `authInterceptor`: functional interceptor que seta `withCredentials: true` e `Content-Type: application/json`
- ✅ Corretamente configurado via `provideHttpClient(withInterceptors([authInterceptor]))`

### Guard
- `authGuard`: functional `CanActivateFn`
- Verifica `authStore.isAuthenticated()` (signal)
- Redireciona para `/login` se não autenticado
- ✅ Bem implementado

### Forms
- Template-driven com `FormsModule` + `[(ngModel)]`
- ❌ Sem ReactiveFormsModule em nenhum componente
- Validação manual (inline) ao invés de Angular validators

### Modern Control Flow
- ⚠️ **Não verificável sem templates** — `.ts` files não mostram se usam `@if/@for` ou `*ngIf/*ngFor`

---

## Gaps Encontrados

1. **Lazy loading: 0%** — nenhum componente usa `loadComponent: () => import(...)`
2. **Signals adoption: 57%** — LoginComponent, CadastroComponent, CadastroPacienteComponent não usam signals
3. **Sem testes** — nenhum `.spec.ts` customizado encontrado
4. **Sem error interceptor** — erros HTTP não são tratados globalmente
5. **Sem loading state global** — cada componente gerencia seu próprio loading
6. **URLs hardcoded** — todos services usam `http://localhost:8080` inline
7. **Dependência suspeita** — `"s": "^1.0.0"` em package.json
8. **MedicalStore** — criado mas pouco utilizado
9. **Dashboard, Laudo-IA, Centro-Laudos** — mostram dados mockados hardcoded
10. **Sem environment files** — sem `environment.ts` / `environment.prod.ts` para API URL

## Referências
- [[wiki/concepts/angular-patterns]]
- [[wiki/concepts/api-endpoints]]
- [[wiki/entities/angular-frontend]]

## Backlinks
- [[wiki/overview]]
- [[context/coding-conventions]]
