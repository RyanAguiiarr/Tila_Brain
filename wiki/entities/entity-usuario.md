---
title: "Entity: Usuario"
type: entity
tags: [backend, jpa, entity, security, lgpd]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Entity: Usuario

## Table
`usuarios`

## Fields
| Field | Type | Annotations | LGPD? |
|---|---|---|---|
| id | UUID | `@Id @GeneratedValue(UUID)` | Não |
| email | String | `@Email @Column(unique, nullable=false)` | ✅ Dado pessoal |
| senha | String | `@Column(nullable=false)` | ✅ Dado sensível (BCrypt hash) |
| perfil | PerfilUser (enum) | `@Enumerated(STRING)` | Não |
| ativo | Boolean | default `true` | Não |
| ultimoAcesso | Timestamp | — | Não |

## Relationships
| Relation | Type | Target | Cascade |
|---|---|---|---|
| — | `@OneToOne` (inverse) | Medico.usuario | Nenhum definido |
| — | `@ManyToOne` (inverse) | LogAuditoria.usuario | Nenhum definido |

## Implementa UserDetails
- `getUsername()` → retorna `email`
- `getPassword()` → retorna `senha`
- `getAuthorities()` → converte `PerfilUser` para `ROLE_*`
  - ADMIN retorna `[ROLE_ADMIN, ROLE_MEDICO]` (hierarquia implementada inline)
- `isEnabled()` → retorna `ativo`

## Observations
- UUID como ID é boa prática para segurança (não sequencial)
- ⚠️ Não há campo `nome` — nome do usuário vem da entidade Medico
- ⚠️ Hierarquia de roles implementada inline em `getAuthorities()` ao invés de `RoleHierarchy` do Spring
- ⚠️ Campo `ultimoAcesso` existe mas não é atualizado em nenhum lugar do código
- ⚠️ Não há `@PrePersist` ou `@CreationTimestamp` — sem tracking de criação

## Backlinks
- [[wiki/entities/entity-medico]]
- [[wiki/entities/entity-log-auditoria]]
- [[wiki/concepts/data-model]]
