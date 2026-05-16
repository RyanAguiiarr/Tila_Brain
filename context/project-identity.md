---
title: "Project Identity — TILA (Verificado)"
type: context
tags: [project, identity, status]
sources: []
last_updated: 2026-05-16
---

# Project Identity — TILA

## Identidade
| Campo | Valor |
|---|---|
| **Nome** | TILA — Tecnologia Integradora de Laudos Automatizados |
| **Integrantes** | Ryan Cantareli de Aguiar, Pedro Henrique Oliveira Pereira |
| **Instituição** | Projeto Integrador ADS — 2026 |
| **Backend** | `c:\Projetos\Tila\Tila_BackEnd\tila\` |
| **Frontend** | `c:\Projetos\Tila\Tila_Frontend\` |
| **Brain** | `c:\Projetos\Tila\Tila_Brain\` |
| **Project Skill** | `c:\Projetos\Tila\tila-project-skill\SKILL.md` |

## Missão
Plataforma assistida por IA que automatiza rascunhos de laudos médicos (pré-laudos) com foco inicial exclusivo em imagens de Raio-X de Tórax, reduzindo tempo de redação e risco de erros por fadiga.

## Stack
- **Backend**: Java 21, Spring Boot 4.0.3, PostgreSQL, LangChain4j 0.36.2
- **Frontend**: Angular 19.2.x, TypeScript 5.7.2, CSS Vanilla
- **IA**: Google Gemini 2.5-flash, text-embedding-004, pgvector

---

## Status Atual (2026-05-07)

### ✅ FEITO
- Autenticação completa (login, registro, JWT, guard)
- CRUD parcial de Pacientes (criar, listar, buscar por ID)
- Logs de auditoria (leitura)
- Entidades JPA: Usuario, Medico, Paciente, Exame, Laudo, LogAuditoria, ConhecimentoMedico
- Frontend: 10 páginas, 4 componentes compartilhados
- Infraestrutura AI: LangChain4j beans (chat, embedding, store, retriever) em `TilaRagConfig`
- Interface `TilaRadiologistaAgent` criada com template `@UserMessage` e `@SystemMessage(fromResource)`
- System prompt `radiologista-system.txt` escrito — foco exclusivo RX Tórax
- Security hardening: env vars para JWT_SECRET, DB_PASSWORD; orElseThrow(); constructor injection

### ⚠️ EM PROGRESSO
- `TilaRadiologistaAgent`: interface criada, mas 3 bugs impedem o funcionamento (ver roadmap)
- Entidade Laudo criada, LaudoService/LaudoController com design completo mas não implementados
- Entidade ConhecimentoMedico criada mas sem CRUD
- Dashboard com dados mockados
- Agenda com dados mockados

### 🔲 PLANEJADO (NÃO INICIADO)
- Exame CRUD completo
- Laudo CRUD + geração por IA (design documentado 2026-05-16)
- Pipeline DICOM → scrub → análise → pré-laudo
- RLHF feedback loop
- Criptografia em repouso (LGPD)
- Deploy (AWS ou equivalente)

### 🐛 BUGS CONHECIDOS (3 críticos)
- `GEMINI_API_KEY` hardcoded em `TilaRagConfig` — regressão introduzida na sessão 2026-05-14
- `radiologista-system.txt` no lugar errado (em `src/main/java` ao invés de `src/main/resources`)
- `@V("imagem")` inválido em `TilaRadiologistaAgent` para tipo `Image`

## Referências
- [[context/roadmap]] — Plano de evolução
- [[context/security-lgpd]] — Estado de segurança
- [[context/ai-pipeline]] — Estado do pipeline de IA
- [[context/coding-conventions]] — Convenções verificadas

## Backlinks
- [[wiki/overview]]
