---
title: "Entity: Exame"
type: entity
tags: [backend, jpa, entity, medical]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Entity: Exame

## Table
`exames`

## Fields
| Field | Type | Annotations | LGPD? |
|---|---|---|---|
| id | Long | `@Id @GeneratedValue(IDENTITY)` | Não |
| tipoExame | String | `@Column(nullable=false)` | Não |
| dataRealização | LocalDateTime | — (⚠️ caracter especial no nome do campo) | Não |
| urlImagem | String | — | ✅ Dado médico sensível |
| status | StatuExame (enum) | `@Enumerated(STRING)`, default PENDENTE | Não |

## Relationships
| Relation | Type | Target | Cascade |
|---|---|---|---|
| paciente | `@ManyToOne` | Paciente | Nenhum |
| medico | `@ManyToOne` | Medico | Nenhum |

## Enums Relacionados
- `StatuExame`: PENDENTE, CONCLUIDO (⚠️ apenas 2 valores — faltam CANCELADO, LAUDADO)

## Observations
- ⚠️ Campo `dataRealização` usa caracter especial (ç) no nome — pode causar problemas em alguns cenários
- ⚠️ `tipoExame` é String livre ao invés de enum — sem validação de valores
- ⚠️ `urlImagem` pode apontar para recurso sem auth — ver [[context/security-lgpd]]
- ⚠️ Sem `@CreationTimestamp`
- ⚠️ StatuExame tem apenas PENDENTE e CONCLUIDO — insuficiente para o workflow completo
- ⚠️ Sem controller ou service para Exame — CRUD não implementado
- ⚠️ FetchType padrão (EAGER) nas relações — risco de performance

## Backlinks
- [[wiki/entities/entity-paciente]]
- [[wiki/entities/entity-medico]]
- [[wiki/entities/entity-laudo]]
- [[wiki/concepts/data-model]]
