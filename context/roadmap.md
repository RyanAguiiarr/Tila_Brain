---
title: "Roadmap — TILA (Verificado)"
type: context
tags: [roadmap, planning, priorities]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-16
---

# Roadmap — TILA

> Baseado na auditoria completa do codebase em 2026-05-07.
> Prioridades derivadas de gaps reais encontrados, não de suposições.

---

## Phase 1 — Consolidation (CURRENT)

> Objetivo: Tornar o que existe robusto e seguro antes de adicionar features.

### 🔴 Security — Prioridade Máxima
- [x] Mover `api.security.token.secret` para variável de ambiente `${JWT_SECRET}`
- [x] Mover `spring.datasource.password` para variável de ambiente `${DB_PASSWORD}`
- [ ] Remover `GEMINI_API_KEY=AIzaSy...` hardcoded do `TilaRagConfig.java` — ❌ **REGRESSÃO** (estava correto, foi reintroduzido)
- [x] Substituir `medico.get()` por `medico.orElseThrow()` em AutenticacaoController
- [x] Substituir `findByEmail(subject).get()` por `orElseThrow()` em SecurityFilter
- [ ] Restringir `GET /logs` para `hasRole("ADMIN")` no SecurityFilterChain

### 🟡 Convention Fixes
- [x] Renomear `logAuditoriaController` → `LogAuditoriaController`
- [x] Renomear `logAuditoriaService` → `LogAuditoriaService`
- [x] Renomear pacote `athenticate` → `authenticate`
- [ ] Corrigir `PacienteResponseDTO` para usar `List<ExameResponseDTO>` ao invés de `List<Exame>`
- [x] Substituir `@Autowired` field injection por constructor injection em: SecurityConfigurations, SecurityFilter, AutenticacaoService, PacienteController
- [ ] Reescrever `GlobalExceptionHandler` para retornar `GenericResult.error()` (não `ErrorDetalhe`)
- [x] Corrigir typos: `bucasPorId` → `buscarPorId`, `bucasTodosPacientes` → `buscarTodosPacientes`

### 🟡 Missing CRUD
- [ ] Implementar `PUT /paciente/{id}` e `DELETE /paciente/{id}` (LGPD direito de retificação/exclusão)
- [ ] Implementar paginação server-side com `Pageable` em: `GET /paciente`, `GET /logs`
- [ ] Popular `ipOrigem` via `HttpServletRequest.getRemoteAddr()` em LogAuditoria

### 🔵 Frontend Quick Wins
- [ ] Implementar lazy loading em `app.routes.ts` (usar `loadComponent: () => import(...)`)
- [ ] Criar `environment.ts` e `environment.prod.ts` para API URL
- [ ] Migrar LoginComponent, CadastroComponent, CadastroPacienteComponent para signals

---

## Phase 2 — Core Features

> Objetivo: Implementar os domínios core que faltam.

### Exame Module
- [ ] Criar `ExameService` com CRUD completo
- [ ] Criar `ExameController` com endpoints: POST, GET (list + by ID), PUT, PATCH status
- [ ] Implementar upload de imagem (local storage → `./uploads/exames`)
- [ ] Conectar com frontend (ProntuarioComponent já espera exames)

### Laudo Module
- [ ] Criar `LaudoService` com: criar, listar por exame, listar por médico, revisar, assinar
- [ ] Criar `LaudoController` com endpoints REST
- [ ] Criar `LaudoRequestDTO` e `LaudoResponseDTO`
- [ ] Conectar com LaudoIaComponent e CentroLaudosComponent no frontend

### Consulta/Agenda Module
- [ ] Definir campos da entidade Consulta (data, hora, tipo, paciente, medico, status)
- [ ] Popular enums StatusConsulta e TipoConsulta
- [ ] Criar ConsultaService e ConsultaController
- [ ] Conectar com AgendaComponent no frontend

### Dashboard
- [ ] Conectar Dashboard com dados reais (contadores, gráficos)
- [ ] Implementar endpoints de estatísticas

---

## Phase 3 — AI Integration

> Objetivo: Ativar o pipeline de IA que já tem infraestrutura pronta.

### Agente Radiologista
- [x] Criar interface `TilaRadiologistaAgent` com `@AiService` e `@SystemMessage(fromResource)`
- [x] Definir system prompt especializado em radiologia (Foco inicial exclusivo: Raio-X de Tórax)
- [ ] ❌ **BUG**: Mover `radiologista-system.txt` de `ai/prompt/` para `resources/prompts/` (classpath)
- [ ] ❌ **BUG**: Corrigir `@Value("AIzaSy...")` hardcoded em `TilaRagConfig` → `@Value("${GEMINI_API_KEY}")`
- [ ] ❌ **BUG**: Remover `@V("imagem")` do parâmetro `Image` em `TilaRadiologistaAgent`
- [x] `ContentRetriever` integrado ao agente via `TilaRagConfig.tilaAgent()`
- [ ] Criar `ExameRepository` com `findByIdWithDetails()` (JOIN FETCH)
- [ ] Criar DTOs: `LaudoGeracaoRequestDTO`, `LaudoResponseDTO`, `LaudoRevisaoRequestDTO`
- [ ] Criar `LaudoService` com: `gerarPreLaudo()`, `revisarLaudo()`, `assinarLaudo()`
- [ ] Criar `LaudoController` com endpoint `POST /laudo/gerar` e demais endpoints REST

### Base de Conhecimento
- [ ] Criar pipeline de ingestão: ConhecimentoMedico → text chunks → embeddings → pgvector
- [ ] Popular com protocolos radiológicos, terminologia, exemplos de laudos
- [ ] Criar CRUD para ConhecimentoMedico (admin-only)

### DICOM (se viável no escopo acadêmico)
- [ ] Implementar leitor DICOM básico (dcm4che ou similar)
- [ ] Implementar scrubbing de metadados LGPD
- [ ] Integrar com upload de exames

---

## Phase 4 — Production Readiness

> Objetivo: Preparar para uso clínico real (se aplicável).

### LGPD Compliance
- [ ] Criptografia em repouso para campos LGPD (AES-256 ou Jasypt)
- [ ] Soft delete para pacientes e laudos
- [ ] Endpoint DSAR (direito de acesso aos dados pessoais)
- [ ] Consentimento explícito do paciente
- [ ] Audit log completo (todas as operações CRUD)

### Infrastructure
- [ ] Migrar de ddl-auto=update para Flyway
- [ ] Criar profiles Spring (dev, staging, prod)
- [ ] Configurar rate limiting (Resilience4j ou similar)
- [ ] Implementar refresh token
- [ ] Adicionar Swagger/OpenAPI
- [ ] Configurar Secure=true no cookie (requer HTTPS)

### Quality
- [ ] Testes unitários (JUnit + Mockito) para services
- [ ] Testes de integração para controllers
- [ ] Testes E2E (Playwright ou Cypress) para fluxos críticos
- [ ] CI/CD pipeline (GitLab CI está configurado mas vazio)

### Deploy
- [ ] Dockerize backend + frontend
- [ ] PostgreSQL + pgvector em container
- [ ] Deploy em cloud (AWS, GCP, ou Railway/Render para acadêmico)

## Referências
- [[context/project-identity]]
- [[context/security-lgpd]]
- [[context/ai-pipeline]]
- [[context/coding-conventions]]

## Backlinks
- [[wiki/overview]]
