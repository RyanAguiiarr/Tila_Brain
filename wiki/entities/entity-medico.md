---
title: "Entity: Medico"
type: entity
tags: [backend, jpa, entity, lgpd]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Entity: Medico

## Table
`medicos`

## Fields
| Field | Type | Annotations | LGPD? |
|---|---|---|---|
| id | Long | `@Id @GeneratedValue(IDENTITY)` | Não |
| nomeCompleto | String | `@Column(nullable=false)` | ✅ Dado pessoal |
| crm | String | `@Column(nullable=false, unique=true)` | ✅ Dado profissional |
| especialidade | String | — | Não |
| assinaturaDigital | String | `@Lob` (Base64) | ✅ Dado pessoal |

## Relationships
| Relation | Type | Target | Cascade |
|---|---|---|---|
| usuario | `@OneToOne` | Usuario | Nenhum |
| — | `@ManyToOne` (inverse) | Exame.medico | Nenhum |
| — | `@ManyToOne` (inverse) | Laudo.medico | Nenhum |

## Observations
- ⚠️ `assinaturaDigital` como `@Lob` String em Base64 — funcional mas pode ter performance issues com imagens grandes
- ⚠️ Sem `@CreationTimestamp` ou `@UpdateTimestamp`
- ✅ CRM unique — impede duplicatas

## Backlinks
- [[wiki/entities/entity-usuario]]
- [[wiki/entities/entity-exame]]
- [[wiki/entities/entity-laudo]]
- [[wiki/concepts/data-model]]
