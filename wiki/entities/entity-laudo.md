---
title: "Entity: Laudo"
type: entity
tags: [backend, jpa, entity, medical, ai]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Entity: Laudo

## Table
`laudo` (nome padrão JPA — sem `@Table`)

## Fields
| Field | Type | Annotations | LGPD? |
|---|---|---|---|
| id | Long | `@Id @GeneratedValue(IDENTITY)` | Não |
| rascunhoIA | String | `@Column(columnDefinition="TEXT")` | ✅ Dado médico sensível |
| textoFinal | String | `@Column(columnDefinition="TEXT")` | ✅ Dado médico sensível |
| achadosJson | String | `@Column(columnDefinition="TEXT")` | ✅ Dado médico sensível |
| impressaoJson | String | `@Column(columnDefinition="TEXT")` | ✅ Dado médico sensível |
| notaIA | String | `@Column(columnDefinition="TEXT")` | Não |
| confidenceScore | Integer | — | Não |
| status | StatusLaudo (enum) | `@Enumerated(STRING) @Column(nullable=false)`, default RASCUNHO | Não |
| hashIntegridade | String | — | Não |
| dataCriacao | LocalDateTime | `@PrePersist` seta automaticamente | Não |
| dataRevisao | LocalDateTime | — | Não |
| dataAssinatura | LocalDateTime | `@PreUpdate` seta quando status=ASSINADO | Não |

## Relationships
| Relation | Type | Target | Cascade |
|---|---|---|---|
| exame | `@ManyToOne(LAZY)` | Exame | `nullable=false` |
| medico | `@ManyToOne(LAZY)` | Medico | `nullable=false` |

## Enums Relacionados
- `StatusLaudo`: RASCUNHO, EM_REVISAO, ASSINADO, CANCELADO

## Observations
- ✅ **EXISTE** — contradiz SKILL.md antigo que dizia "Laudo entity does not exist"
- ✅ Usa `@PrePersist` e `@PreUpdate` para timestamps automáticos
- ✅ `FetchType.LAZY` nas relações — boa prática
- ✅ `hashIntegridade` previsto para integridade do laudo (ainda sem implementação)
- ⚠️ `@PreUpdate` tem bug lógico: atualiza `dataAssinatura` em AMBOS os branches do if
- ⚠️ Sem `@Table` — usa nome padrão da classe
- ⚠️ Sem controller ou service — CRUD não implementado
- ⚠️ Constructor com 14 parâmetros — deveria usar builder ou factory method
- ⚠️ `achadosJson` e `impressaoJson` como String TEXT — sem tipagem ou validação de JSON

## Backlinks
- [[wiki/entities/entity-exame]]
- [[wiki/entities/entity-medico]]
- [[wiki/concepts/data-model]]
- [[context/ai-pipeline]]
