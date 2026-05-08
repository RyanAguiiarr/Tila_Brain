---
title: "Entity: LogAuditoria"
type: entity
tags: [backend, jpa, entity, security, audit]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Entity: LogAuditoria

## Table
`logs_auditoria`

## Fields
| Field | Type | Annotations | LGPD? |
|---|---|---|---|
| id | Long | `@Id @GeneratedValue(IDENTITY)` | Não |
| acao | String | `@Column(nullable=false)` | Não |
| dataHora | LocalDateTime | `@Column(nullable=false)` | Não |
| ipOrigem | String | — | Não |

## Relationships
| Relation | Type | Target | Cascade |
|---|---|---|---|
| usuario | `@ManyToOne` | Usuario | Nenhum |

## Ações Registradas (no código)
- `CADASTRO_NOVO_MEDICO` — ao registrar médico
- `LOGIN_SUCESSO` — ao fazer login
- `CADASTRO_PACIENTE` — ao cadastrar paciente
- `CONSULTA_PACIENTE_CPF` — ao buscar paciente por CPF
- `CONSULTA_PACIENTE_ID` — ao buscar paciente por ID

## Queries Customizadas (Repository)
- `findByUsuarioIdOrderByDataHoraDesc(UUID)` — logs de um usuário
- `findByDataHoraBetween(LocalDateTime, LocalDateTime)` — logs por período

## Observations
- ⚠️ `ipOrigem` **NUNCA É POPULADO** em nenhum lugar do código — sempre null
- ⚠️ Sem ações de DELETE, UPDATE — apenas criação e leitura são logadas
- ⚠️ logAuditoriaService e logAuditoriaController violam naming convention Java (camelCase ao invés de PascalCase)
- ⚠️ `buscarTodosOsLogs()` usa `findAll()` sem paginação — carrega TODOS os logs
- ⚠️ Retorna `List<LogAuditoria>` (entity) ao invés de DTO — expõe dados internos
- ⚠️ A lógica de criação de log está espalhada: inline no controller (Auth) e via helper method no service (Paciente) — sem padrão consistente

## Backlinks
- [[wiki/entities/entity-usuario]]
- [[wiki/concepts/data-model]]
- [[context/security-lgpd]]
