---
title: "SKILL.md Update — 2026-05-07"
type: output
tags: [skill, update, comparison]
sources: []
last_updated: 2026-05-07
---

# SKILL.md Update — Comparação Antes/Depois

> Gerado na ingestão inicial do codebase em 2026-05-07.

---

## O que estava CORRETO no SKILL.md antigo

| Item | Status |
|---|---|
| Java 21, Spring Boot 4.0.3 | ✅ Confirmado |
| Auth0 java-jwt 4.4.0 | ✅ Confirmado |
| Angular 19.2.x, TypeScript 5.7.x | ✅ Confirmado |
| Estrutura de pacotes do backend | ✅ Confirmado (com adições) |
| Endpoints /auth/registrar, /auth/login, /auth/me | ✅ Confirmado |
| GenericResult<T> pattern | ✅ Confirmado |
| JWT HMAC256, HttpOnly cookie `accessToken` | ✅ Confirmado |
| BCrypt, STATELESS sessions | ✅ Confirmado |
| Roles: MEDICO, PACIENTE, ADMIN | ✅ Confirmado |
| Standalone components 100% | ✅ Confirmado |
| Signal-based AuthStore | ✅ Confirmado |
| Template-driven forms | ✅ Confirmado |
| CSS Vanilla, sem Tailwind | ✅ Confirmado |
| ipOrigem nunca populado | ✅ Confirmado |
| Secret JWT hardcoded "Cucamole@123" | ✅ Confirmado (ainda presente) |
| Senha DB hardcoded | ✅ Confirmado (ainda presente) |
| `medico.get()` sem verificação | ✅ Confirmado (ainda presente) |
| Typo `athenticate` no pacote | ✅ Confirmado (ainda presente) |
| Typo `logAuditoriaController` | ✅ Confirmado (ainda presente) |

---

## O que estava ERRADO ou DESATUALIZADO

| Item no SKILL.md antigo | Realidade atual |
|---|---|
| DB URL: `localhost:5433/TilaDB` | **Mudou** para `localhost:5434/vectorDB` |
| "Laudo entity **não existe** ainda" | **EXISTE** — com 14 campos, StatusLaudo enum, @PrePersist/@PreUpdate |
| Sem menção a LangChain4j | **Adicionado**: langchain4j 0.36.2 BOM, Google Gemini, pgvector |
| Sem menção a ConhecimentoMedico | **Nova entidade** com CategoriaConhecimento enum |
| Sem menção a TilaRagConfig | **Novo** — configuração completa de RAG (ChatModel, EmbeddingModel, EmbeddingStore, ContentRetriever) |
| Sem menção a StatusLaudo enum | **Novo**: RASCUNHO, EM_REVISAO, ASSINADO, CANCELADO |
| Sem menção a CategoriaConhecimento enum | **Novo**: PROTOCOLO, ANATOMIA, ACR_BIRADS, etc. |
| Sem menção a LaudoRepository | **Novo**: findByMedicoAndStatus, findByExameId |
| Sem menção a ConhecimentoMedicoRepository | **Novo** |
| Sem menção a OAuth2 Authorization Server starter | **Presente** no pom.xml (mas não usado ativamente) |
| Sem menção a Gemini API key hardcoded | **Presente** em application.properties |
| Sem menção a upload config (tila.upload.path) | **Presente** em application.properties |
| "Bun" como package manager | **Não confirmado** — package.json padrão npm |

---

## O que é NOVO no SKILL.md atualizado (nunca documentado antes)

| Item | Detalhes |
|---|---|
| AI infrastructure status | TilaRagConfig com 4 beans configurados, pipeline 70% infra 0% operacional |
| Laudo entity completa | 14 campos, @PrePersist, @PreUpdate, StatusLaudo enum |
| ConhecimentoMedico entity | Base de conhecimento para RAG, 7 categorias |
| 23 issues catalogados | 6 critical, 11 medium, 6 low — priorizados |
| 14 divergências de convenção | Cada uma com arquivo, convenção esperada vs encontrada |
| LGPD exposure map | 14 campos sensíveis, 13 sem proteção |
| ADR-002 (GenericResult) | Compliance snapshot: 85% happy path, 0% error path |
| ADR-003 (Security Architecture) | 6 decisões tomadas, 8 decisões pendentes |
| Frontend compliance metrics | 100% standalone, 57% signals, 0% lazy loading |
| Realistic AI roadmap | Honest "infrastructure ready, orchestration missing" |

## Backlinks
- [[wiki/overview]]
