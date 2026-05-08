---
title: "Entity: Paciente"
type: entity
tags: [backend, jpa, entity, lgpd]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Entity: Paciente

## Table
`pacientes`

## Fields
| Field | Type | Annotations | LGPD? |
|---|---|---|---|
| id | Long | `@Id @GeneratedValue(IDENTITY)` | Não |
| nomeCompleto | String | `@Column(nullable=false)` | ✅ Dado pessoal |
| cpf | String | `@Column(unique=true, nullable=false)` | ✅ Dado pessoal sensível |
| dataNascimento | LocalDate | — | ✅ Dado pessoal |
| telefone | String | — | ✅ Dado pessoal |

## Relationships
| Relation | Type | Target | Cascade |
|---|---|---|---|
| exames | `@OneToMany(mappedBy="paciente")` | Exame | Nenhum |

## Observations
- ⚠️ 4 campos LGPD sensíveis — CPF é o mais crítico
- ⚠️ Sem `@CreationTimestamp` ou `@UpdateTimestamp`
- ⚠️ Sem soft delete (delete é permanente)
- ⚠️ Entity importa `PacienteRequestDTO` — violação de separação de camadas (entity não deveria conhecer DTOs)
- ⚠️ `PacienteResponseDTO` retorna `List<Exame>` diretamente ao invés de `List<ExameResponseDTO>` — risco de serialização circular
- ✅ CPF unique — impede duplicatas
- ✅ `existsByCpf()` no repository previne cadastro duplicado

## Backlinks
- [[wiki/entities/entity-exame]]
- [[wiki/concepts/data-model]]
