---
title: "Modelo de Dados — TILA"
type: concept
tags: [backend, database, jpa, data-model, lgpd]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Modelo de Dados — TILA

## Visão Geral do ER

O TILA possui **7 entidades JPA**, das quais 4 são funcionais, 2 são criadas mas sem CRUD, e 1 é placeholder vazio.

### Grafo de Relacionamentos

```
Usuario (UUID) ←─ 1:1 ──→ Medico (Long)
    │                         │
    │ 1:N                     │ 1:N (Exame.medico)
    ↓                         ↓
LogAuditoria (Long)       Exame (Long) ←── N:1 ── Paciente (Long)
                              │                       │
                              │ 1:N (Laudo.exame)     │ 1:N (Paciente.exames)
                              ↓                       │
                          Laudo (Long)                 │
                              │                       ↓
                              └── N:1 ── Medico       [exames list]

ConhecimentoMedico (Long) — STANDALONE (sem relações)
Consulta — VAZIA
```

### Status por Entidade
| Entidade | Controller | Service | Repository | CRUD Completo? |
|---|---|---|---|---|
| Usuario | ✅ (via Auth) | ✅ (AutenticacaoService) | ✅ | Parcial (sem CRUD direto) |
| Medico | ❌ | ❌ | ✅ | ❌ (criado via Auth) |
| Paciente | ✅ | ✅ | ✅ | Parcial (sem PUT/DELETE) |
| Exame | ❌ | ❌ | ❌ (sem custom queries) | ❌ |
| Laudo | ❌ | ❌ | ✅ | ❌ |
| LogAuditoria | ✅ | ✅ | ✅ | Apenas leitura |
| ConhecimentoMedico | ❌ | ❌ | ✅ | ❌ |
| Consulta | ❌ | ❌ | ❌ (vazio) | ❌ |

---

## Mapa de Exposição LGPD

### Dados Pessoais Identificáveis
| Entidade | Campo | Tipo LGPD | Proteção Atual |
|---|---|---|---|
| Usuario | email | Dado pessoal | ❌ Sem criptografia em repouso |
| Usuario | senha | Dado sensível | ✅ BCrypt hash |
| Medico | nomeCompleto | Dado pessoal | ❌ Texto plano |
| Medico | crm | Dado profissional | ❌ Texto plano |
| Medico | assinaturaDigital | Dado pessoal biométrico | ❌ Base64 sem criptografia |
| Paciente | nomeCompleto | Dado pessoal | ❌ Texto plano |
| Paciente | cpf | Dado pessoal sensível | ❌ Texto plano |
| Paciente | dataNascimento | Dado pessoal | ❌ Texto plano |
| Paciente | telefone | Dado pessoal | ❌ Texto plano |

### Dados Médicos Sensíveis (Art. 11 LGPD)
| Entidade | Campo | Tipo LGPD | Proteção Atual |
|---|---|---|---|
| Laudo | rascunhoIA | Dado de saúde | ❌ Texto plano |
| Laudo | textoFinal | Dado de saúde | ❌ Texto plano |
| Laudo | achadosJson | Dado de saúde | ❌ Texto plano |
| Laudo | impressaoJson | Dado de saúde | ❌ Texto plano |
| Exame | urlImagem | Referência a dado de saúde | ❌ URL sem auth |

### Resumo de Risco LGPD
- **Total de campos LGPD sensíveis**: 14
- **Campos com proteção**: 1 (senha com BCrypt)
- **Campos sem proteção**: 13
- **Nível de conformidade**: 🔴 **BAIXO**

---

## Gaps Encontrados

### Auditoria
- ⚠️ Nenhuma entidade tem `@CreatedBy` ou `@LastModifiedBy`
- ⚠️ `LogAuditoria.ipOrigem` nunca é populado
- ⚠️ Sem `@CreationTimestamp`/`@UpdateTimestamp` em Usuario, Medico, Paciente, Exame
- ✅ Laudo e ConhecimentoMedico usam `@PrePersist`/`@PreUpdate`

### Soft Delete
- ⚠️ Nenhuma entidade implementa soft delete
- ⚠️ `Usuario.ativo` existe mas delete físico não é impedido
- ⚠️ LGPD requer capacidade de "anonimização" — delete físico pode ser insuficiente

### Integridade Referencial
- ⚠️ Nenhum cascade configurado — deleção de entidade pai pode falhar silenciosamente
- ⚠️ `PacienteResponseDTO` retorna `List<Exame>` (entity JPA) ao invés de DTO — risco de LazyInitializationException e serialização circular

### DDL
- ⚠️ `spring.jpa.hibernate.ddl-auto=update` — perigoso em produção
- ⚠️ Sem Flyway ou Liquibase — sem versionamento de schema

## Referências
- [[wiki/entities/entity-usuario]]
- [[wiki/entities/entity-medico]]
- [[wiki/entities/entity-paciente]]
- [[wiki/entities/entity-exame]]
- [[wiki/entities/entity-laudo]]
- [[wiki/entities/entity-log-auditoria]]
- [[wiki/entities/entity-conhecimento-medico]]
- [[wiki/entities/entity-consulta]]
- [[context/security-lgpd]]

## Backlinks
- [[wiki/entities/spring-boot-backend]]
- [[wiki/overview]]
