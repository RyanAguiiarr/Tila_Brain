# Tila_Brain — Index
> v2 architecture. Last updated: 2026-06-24.

## Core files
- [CLAUDE.md](file:///c:/Projetos/Tila/Tila_Brain/CLAUDE.md) — Agent operating manual (start here)
- [SOUL.md](file:///c:/Projetos/Tila/Tila_Brain/00-Index/SOUL.md) — Agent identity and non-negotiables
- [GuiaSkillsCerebro.md](file:///c:/Projetos/Tila/Tila_Brain/00-Index/GuiaSkillsCerebro.md) — Guia de todas as skills do cérebro
- [GuiaUsoAgenteIDE.md](file:///c:/Projetos/Tila/Tila_Brain/00-Index/GuiaUsoAgenteIDE.md) — Como usar o cérebro com o agente da IDE (manual e automático)
- [manual-contexto-graphify.md](file:///c:/Projetos/Tila/Tila_Brain/00-Index/manual-contexto-graphify.md) — Guia definitivo do Graphify (omnipresente no ciclo de desenvolvimento)
- [index.md](file:///c:/Projetos/Tila/Tila_Brain/index.md) — This file
- [log.md](file:///c:/Projetos/Tila/Tila_Brain/log.md) — Append-only activity log

---

## Context (mutable, latest state)
- [project-identity.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/contexto/project-identity.md) — Stack, team, repos
- [coding-conventions.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/contexto/coding-conventions.md) — Confirmed + violated conventions
- [security-lgpd.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/contexto/security-lgpd.md) — Security gaps and LGPD status
- [ai-pipeline.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/contexto/ai-pipeline.md) — AI pipeline honest status
- [roadmap.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/contexto/roadmap.md) — Phased roadmap from audit

---

## Negócio — Permanent Knowledge

### Decisões (ADRs)
- [ADR-001](file:///c:/Projetos/Tila/Tila_Brain/02-Arquitetura_ADRs/ADR-001-jwt-httponly-cookie-protege-contra-xss.md) — JWT via HttpOnly cookie protege contra XSS
- [ADR-002](file:///c:/Projetos/Tila/Tila_Brain/02-Arquitetura_ADRs/ADR-002-genericresult-envelope-universal.md) — GenericResult<T> como envelope universal
- [ADR-003](file:///c:/Projetos/Tila/Tila_Brain/02-Arquitetura_ADRs/ADR-003-java-records-imutaveis-dtos.md) — Java records imutáveis para DTOs
- [ADR-004](file:///c:/Projetos/Tila/Tila_Brain/02-Arquitetura_ADRs/ADR-004-angular-standalone-sem-ngmodule.md) — Angular standalone sem NgModule
- [ADR-005](file:///c:/Projetos/Tila/Tila_Brain/02-Arquitetura_ADRs/ADR-005-langchain4j-gemini-stack-ia.md) — LangChain4j + Gemini como stack IA
- [ADR-006](file:///c:/Projetos/Tila/Tila_Brain/02-Arquitetura_ADRs/ADR-006-pgvector-postgresql-embeddings.md) — pgvector para embeddings

### Domínio Médico
- [laudo-medico-brasileiro-tem-5-secoes-obrigatorias.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/medico/laudo-medico-brasileiro-tem-5-secoes-obrigatorias.md)
- [laudo-ia-exige-revisao-humana-obrigatoria.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/medico/laudo-ia-exige-revisao-humana-obrigatoria.md)

### Produto
- [tila-resolve-gargalo-de-laudos-manuais.md](file:///c:/Projetos/Tila/Tila_Brain/01-Negocio/produto/tila-resolve-gargalo-de-laudos-manuais.md)

### MOCs (Maps of Content)
- [moc-seguranca-lgpd.md](file:///c:/Projetos/Tila/Tila_Brain/00-Index/mocs/moc-seguranca-lgpd.md) — Segurança e LGPD
- [moc-pipeline-ia.md](file:///c:/Projetos/Tila/Tila_Brain/00-Index/mocs/moc-pipeline-ia.md) — Pipeline de IA
- [moc-laudo-medico.md](file:///c:/Projetos/Tila/Tila_Brain/00-Index/mocs/moc-laudo-medico.md) — Laudo Médico

---

## Codebase — Code Knowledge

### Snapshots (immutable audits)
- [backend-audit-2026-06-09.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/snapshots/backend-audit-2026-06-09.md) — Complete backend (entities, endpoints, services, DTOs)
- [backend-security-2026-06-09.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/snapshots/backend-security-2026-06-09.md) — Security & LGPD (13 gaps, 13 LGPD exposures)
- [backend-ai-agent-2026-06-09.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/snapshots/backend-ai-agent-2026-06-09.md) — AI pipeline (85% orchestration, 30% complete)
- [frontend-audit-2026-06-09.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/snapshots/frontend-audit-2026-06-09.md) — Complete frontend (10 pages, 5 components, core services)

### Patterns (verified coding patterns)
- [padrão-genericresult.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/patterns/padrão-genericresult.md) — ResponseEntity<GenericResult<T>> envelope
- [padrão-injecao-construtor.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/patterns/padrão-injecao-construtor.md) — Constructor injection rule
- [padrão-dto-records.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/patterns/padrão-dto-records.md) — Java records for DTOs
- [padrão-seguranca-jwt.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/patterns/padrão-seguranca-jwt.md) — JWT security flow
- [padrão-componente-standalone.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/patterns/padrão-componente-standalone.md) — Angular standalone components
- [padrão-signals-estado.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/patterns/padrão-signals-estado.md) — Angular Signals state management
- [padrão-css-vanilla.md](file:///c:/Projetos/Tila/Tila_Brain/03-Codebase/patterns/padrão-css-vanilla.md) — CSS vanilla styling

---

## Skills (operational)

### Session Pipeline (Fluxo do Programador)
- [skill-session-boot.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-session-boot.md) — Loads brain state at session start (/boot)
- [skill-session-recorder.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-session-recorder.md) — Records EVERYTHING during programming (automatic)
- [skill-arch-review.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-arch-review.md) — Analyzes ideas before coding (/arch-review)
- [skill-session-close.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-session-close.md) — Closes session, processes, commits (/close, /salve)
- [skill-brain-organizer.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-brain-organizer.md) — Organizes brain before commit (/organizar)

### Knowledge & Governance
- [skill-gate-validacao.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-gate-validacao.md) — 6-question gate for permanent notes
- [skill-graphify-query.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-graphify-query.md) — **Omnipresent context engine:** diagnoses bugs, creates features, refactors, migrates AND blast radius
- [skill-dev-assistant.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-dev-assistant.md) — Active coding governance layer
- [skill-moc-update.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-moc-update.md) — Update MOCs after new notes
- [skill-capture-feature.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-capture-feature.md) — After every feature (/capture)
- [skill-ingest.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-ingest.md) — Ingest new knowledge sources (/ingest)
- [skill-generate-laudo.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-generate-laudo.md) — Pré-laudo generation
- [skill-adr.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-adr.md) — Write ADRs
- [skill-query.md](file:///c:/Projetos/Tila/Tila_Brain/05-Skills_Agentes/skill-query.md) — Query the wiki with confidence score

---

## Stats
- Permanent notes: 9 (6 ADRs + 2 medical + 1 product)
- Patterns: 7
- Snapshots: 4
- MOCs: 3
- Skills: 18 (5 session pipeline + 13 knowledge/governance)
- Architecture version: v2.1 (Breno + Okamoto + Karpathy + Session Pipeline)
