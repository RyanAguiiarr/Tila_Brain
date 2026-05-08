---
title: "Project Identity — TILA (Verificado)"
type: context
tags: [project, identity, status]
sources: []
last_updated: 2026-05-07
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
Plataforma assistida por IA que automatiza rascunhos de laudos médicos (pré-laudos) a partir de imagens DICOM e notas clínicas, reduzindo tempo de redação e risco de erros por fadiga.

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
- Infraestrutura AI: LangChain4j beans (chat, embedding, store, retriever)

### ⚠️ EM PROGRESSO
- Entidade Laudo criada mas sem controller/service
- Entidade ConhecimentoMedico criada mas sem CRUD
- Dashboard com dados mockados
- Agenda com dados mockados

### 🔲 PLANEJADO (NÃO INICIADO)
- Exame CRUD completo
- Laudo CRUD + geração por IA
- TilaRadiologistaAgent (interface LangChain4j)
- Pipeline DICOM → scrub → análise → pré-laudo
- RLHF feedback loop
- Criptografia em repouso (LGPD)
- Deploy (AWS ou equivalente)

### 🐛 BUGS CONHECIDOS
- `medico.get()` sem verificação — crash se médico não encontrado
- `findByEmail().get()` no SecurityFilter — crash se usuário deletado
- `ipOrigem` nunca populado nos logs
- GlobalExceptionHandler retorna formato diferente de GenericResult

## Referências
- [[context/roadmap]] — Plano de evolução
- [[context/security-lgpd]] — Estado de segurança
- [[context/ai-pipeline]] — Estado do pipeline de IA
- [[context/coding-conventions]] — Convenções verificadas

## Backlinks
- [[wiki/overview]]
