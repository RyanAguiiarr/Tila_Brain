# Tila_Brain — Index
> v2 architecture. Last updated: 2026-06-09.

## Core files
- [CLAUDE.md](file:///c:/Tila/Tila_Brain/CLAUDE.md) — Agent operating manual (start here)
- [SOUL.md](file:///c:/Tila/Tila_Brain/context/SOUL.md) — Agent identity and non-negotiables
- [index.md](file:///c:/Tila/Tila_Brain/index.md) — This file
- [log.md](file:///c:/Tila/Tila_Brain/log.md) — Append-only activity log

---

## Context (mutable, latest state)
- [project-identity.md](file:///c:/Tila/Tila_Brain/context/project-identity.md) — Stack, team, repos
- [coding-conventions.md](file:///c:/Tila/Tila_Brain/context/coding-conventions.md) — Confirmed + violated conventions
- [security-lgpd.md](file:///c:/Tila/Tila_Brain/context/security-lgpd.md) — Security gaps and LGPD status
- [ai-pipeline.md](file:///c:/Tila/Tila_Brain/context/ai-pipeline.md) — AI pipeline honest status
- [roadmap.md](file:///c:/Tila/Tila_Brain/context/roadmap.md) — Phased roadmap from audit

---

## Negócio — Permanent Knowledge

### Decisões (ADRs)
- [ADR-001](file:///c:/Tila/Tila_Brain/negocio/permanent/decisoes/ADR-001-jwt-httponly-cookie-protege-contra-xss.md) — JWT via HttpOnly cookie protege contra XSS
- [ADR-002](file:///c:/Tila/Tila_Brain/negocio/permanent/decisoes/ADR-002-genericresult-envelope-universal.md) — GenericResult<T> como envelope universal
- [ADR-003](file:///c:/Tila/Tila_Brain/negocio/permanent/decisoes/ADR-003-java-records-imutaveis-dtos.md) — Java records imutáveis para DTOs
- [ADR-004](file:///c:/Tila/Tila_Brain/negocio/permanent/decisoes/ADR-004-angular-standalone-sem-ngmodule.md) — Angular standalone sem NgModule
- [ADR-005](file:///c:/Tila/Tila_Brain/negocio/permanent/decisoes/ADR-005-langchain4j-gemini-stack-ia.md) — LangChain4j + Gemini como stack IA
- [ADR-006](file:///c:/Tila/Tila_Brain/negocio/permanent/decisoes/ADR-006-pgvector-postgresql-embeddings.md) — pgvector para embeddings

### Domínio Médico
- [laudo-medico-brasileiro-tem-5-secoes-obrigatorias.md](file:///c:/Tila/Tila_Brain/negocio/permanent/medico/laudo-medico-brasileiro-tem-5-secoes-obrigatorias.md)
- [laudo-ia-exige-revisao-humana-obrigatoria.md](file:///c:/Tila/Tila_Brain/negocio/permanent/medico/laudo-ia-exige-revisao-humana-obrigatoria.md)

### Produto
- [tila-resolve-gargalo-de-laudos-manuais.md](file:///c:/Tila/Tila_Brain/negocio/permanent/produto/tila-resolve-gargalo-de-laudos-manuais.md)

### MOCs (Maps of Content)
- [moc-seguranca-lgpd.md](file:///c:/Tila/Tila_Brain/negocio/mocs/moc-seguranca-lgpd.md) — Segurança e LGPD
- [moc-pipeline-ia.md](file:///c:/Tila/Tila_Brain/negocio/mocs/moc-pipeline-ia.md) — Pipeline de IA
- [moc-laudo-medico.md](file:///c:/Tila/Tila_Brain/negocio/mocs/moc-laudo-medico.md) — Laudo Médico

---

## Codebase — Code Knowledge

### Snapshots (immutable audits)
- [backend-audit-2026-06-09.md](file:///c:/Tila/Tila_Brain/codebase/snapshots/backend-audit-2026-06-09.md) — Complete backend (entities, endpoints, services, DTOs)
- [backend-security-2026-06-09.md](file:///c:/Tila/Tila_Brain/codebase/snapshots/backend-security-2026-06-09.md) — Security & LGPD (13 gaps, 13 LGPD exposures)
- [backend-ai-agent-2026-06-09.md](file:///c:/Tila/Tila_Brain/codebase/snapshots/backend-ai-agent-2026-06-09.md) — AI pipeline (85% orchestration, 30% complete)
- [frontend-audit-2026-06-09.md](file:///c:/Tila/Tila_Brain/codebase/snapshots/frontend-audit-2026-06-09.md) — Complete frontend (10 pages, 5 components, core services)

### Patterns (verified coding patterns)
- [padrão-genericresult.md](file:///c:/Tila/Tila_Brain/codebase/patterns/padr%C3%A3o-genericresult.md) — ResponseEntity<GenericResult<T>> envelope
- [padrão-injecao-construtor.md](file:///c:/Tila/Tila_Brain/codebase/patterns/padr%C3%A3o-injecao-construtor.md) — Constructor injection rule
- [padrão-dto-records.md](file:///c:/Tila/Tila_Brain/codebase/patterns/padr%C3%A3o-dto-records.md) — Java records for DTOs
- [padrão-seguranca-jwt.md](file:///c:/Tila/Tila_Brain/codebase/patterns/padr%C3%A3o-seguranca-jwt.md) — JWT security flow
- [padrão-componente-standalone.md](file:///c:/Tila/Tila_Brain/codebase/patterns/padr%C3%A3o-componente-standalone.md) — Angular standalone components
- [padrão-signals-estado.md](file:///c:/Tila/Tila_Brain/codebase/patterns/padr%C3%A3o-signals-estado.md) — Angular Signals state management
- [padrão-css-vanilla.md](file:///c:/Tila/Tila_Brain/codebase/patterns/padr%C3%A3o-css-vanilla.md) — CSS vanilla styling

---

## Skills (operational)
- [skill-gate-validacao.md](file:///c:/Tila/Tila_Brain/skills/skill-gate-validacao.md) — 6-question gate for permanent notes
- [skill-graphify-query.md](file:///c:/Tila/Tila_Brain/skills/skill-graphify-query.md) — Blast radius before code changes
- [skill-moc-update.md](file:///c:/Tila/Tila_Brain/skills/skill-moc-update.md) — Update MOCs after new notes
- [skill-capture-feature.md](file:///c:/Tila/Tila_Brain/skills/skill-capture-feature.md) — After every feature (/capture)
- [skill-generate-laudo.md](file:///c:/Tila/Tila_Brain/skills/skill-generate-laudo.md) — Pré-laudo generation
- [skill-adr.md](file:///c:/Tila/Tila_Brain/skills/skill-adr.md) — Write ADRs

---

## Stats
- Permanent notes: 9 (6 ADRs + 2 medical + 1 product)
- Patterns: 7
- Snapshots: 4
- MOCs: 3
- Skills: 6+
- Architecture version: v2 (Breno + Okamoto + Karpathy method)
