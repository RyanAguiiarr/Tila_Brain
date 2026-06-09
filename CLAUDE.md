# CLAUDE.md — Tila_Brain v2
> Rewritten: 2026-06-09
> This file governs all agent behavior in Tila_Brain.
> Read it completely before every session. No exceptions.

## On session start
1. Read this file completely
2. Read context/SOUL.md — internalize before doing anything
3. Read index.md — understand current state of the brain
4. Read log.md last 10 entries — understand what happened recently
5. Read context/roadmap.md — understand current priorities
6. Announce: "Tila_Brain v2 loaded. [N] permanent notes. [N] patterns. Last: [date]."

---

## §1 — Identity
You are the Tila agent. Read context/SOUL.md now.
Your non-negotiables are defined there. They override everything else.

---

## §2 — Business knowledge layer (Breno method)

### Where knowledge lives
- `negocio/inbox/` — raw, unvalidated notes. Agent NEVER reads these as truth.
- `negocio/permanent/` — validated knowledge. Agent trusts these.
- `negocio/mocs/` — navigation maps. Read before exploring a cluster.

### Rules for this layer
- NEVER write to negocio/permanent/ without running skill-gate-validacao first
- Titles must be theses (assertions), never labels
- Every permanent note must link to at least one other permanent note
- AI writes DRAFTS. Human promotes drafts to permanent after review.
- "The density of the graph is not intelligence." — Breno Vieira

### Writing a new permanent note
1. Write draft in negocio/inbox/[slug]-DRAFT.md
2. Run skill-gate-validacao
3. If gate passes: move to appropriate permanent/ subfolder, update index.md
4. If gate fails: append ## Gate Failures section with specific fixes needed
5. Run skill-moc-update after any batch of new permanent notes

---

## §3 — Code knowledge layer (Breno method)

### Where code knowledge lives
- `codebase/snapshots/` — point-in-time audits. Immutable once written.
- `codebase/patterns/` — verified coding patterns as permanent thesis notes.
- `codebase/changelog/` — feature capture log (written by /capture).

### Rules for this layer
- ALWAYS run skill-graphify-query before suggesting any code change
- NEVER suggest modifying a class without knowing its dependents
- Patterns in codebase/patterns/ must be verified in real files — no assumptions
- After every feature: /capture → gate on extracted patterns → graphify rebuild

### Code query protocol
Question about code → skill-graphify-query → blast radius → then answer
Question about architecture → read relevant ADR in negocio/permanent/decisoes/
Question about "what exists" → read codebase/snapshots/ most recent audit

---

## §4 — Operation layer (Okamoto method)

### Skills (how I act)
See skills/ for all available skills.
Key skills:
- skill-gate-validacao — before ANY permanent note
- skill-graphify-query — before ANY code change
- skill-capture-feature (/capture) — after every feature
- skill-generate-laudo — for pré-laudo generation
- skill-adr — for architectural decisions
- skill-moc-update — after new permanent notes

### Crons (when I act autonomously)
See crons/_cron-registry.md for the full schedule.
Key crons: weekly-digest (Monday), lint-wiki (Sunday), gate-inbox (Monday)

### Session rhythm
Open → boot (git pull) → work → /capture after each feature → /salve to close

---

## §5 — Learning layer (Karpathy method)

### Where raw knowledge lives
- `raw/` — immutable source documents. Agent reads, never modifies.
  - raw/articles/ — web articles
  - raw/videos/ — YouTube transcripts
  - raw/laudos/ — anonymized medical reports
  - raw/assets/ — images

### Ingest protocol (/ingest [source])
1. Read the source in raw/
2. Discuss 2-3 key takeaways with the human BEFORE writing anything
3. Propose draft notes for negocio/inbox/ (human approves)
4. Run skill-gate-validacao on each proposed note
5. Update index.md and log.md after any promotion to permanent/

### Index and log conventions
index.md — catalog of ALL permanent notes + patterns + ADRs + snapshots
log.md — append-only. Format: ## [YYYY-MM-DD HH:MM] action | description

---

## §6 — Core constraints (never violate)

1. Never modify files in raw/ — immutable source truth
2. Never write to negocio/permanent/ without gate passing
3. Never suggest code changes without checking blast radius first
4. Never expose real patient data — always synthetic in examples
5. Never invent medical facts — mark as "⚠️ Unverified — source needed"
6. Never ignore an ADR — if a decision conflicts with an existing ADR, flag it
7. Always update index.md and log.md after any wiki change
8. LGPD is a hard constraint — flag every violation, never rationalize it

---

## TILA-Specific Coding Rules (quick reference)

### Backend
- All responses: `ResponseEntity<GenericResult<T>>`
- DTOs: Java records with Bean Validation
- Services: constructor injection only (no @Autowired field)
- Write operations: @Transactional required
- Read operations: @Transactional(readOnly=true)
- Passwords: BCryptPasswordEncoder
- Secrets: environment variables only — NEVER in application.properties

### Frontend
- All components: standalone (no NgModule)
- State: Angular Signals (via stores)
- DI: inject() function
- Guards/interceptors: functional
- CSS: vanilla only — never Tailwind
- API calls: withCredentials=true (cookie transport)

### Medical domain
- AI generates DRAFTS — human always reviews
- Never store real patient data in examples
- Laudos must include "human review required" disclaimer
- LGPD compliance is non-negotiable

## Paths (verified 2026-06-09)

| Repo | Path |
|---|---|
| Brain root | `c:\Tila\Tila_Brain` |
| Backend | `c:\Tila\Tila_BackEnd` |
| Frontend | `c:\Tila\Tila_Frontend` |
