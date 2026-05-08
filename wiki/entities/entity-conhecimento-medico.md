---
title: "Entity: ConhecimentoMedico"
type: entity
tags: [backend, jpa, entity, ai, rag]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Entity: ConhecimentoMedico

## Table
`conhecimento_medico`

## Fields
| Field | Type | Annotations | LGPD? |
|---|---|---|---|
| id | Long | `@Id @GeneratedValue(IDENTITY)` | Não |
| titulo | String | `@Column(nullable=false)` | Não |
| conteudo | String | `@Column(columnDefinition="TEXT", nullable=false)` | Não |
| categoriaConhecimento | CategoriaConhecimento (enum) | `@Enumerated(STRING) @Column(nullable=false)` | Não |
| tipoExameRelacionado | String | — (Ex: RX_TORAX) | Não |
| regiaoAnatomica | String | — (Ex: TORAX, CRANIO) | Não |
| dataCriacao | LocalDateTime | `@Column(nullable=false, updatable=false)` + `@PrePersist` | Não |
| dataAtualizacao | LocalDateTime | `@PreUpdate` | Não |

## Relationships
Nenhuma relação definida — entidade autônoma.

## Enums Relacionados
- `CategoriaConhecimento`: PROTOCOLO, ANATOMIA, ACR_BIRADS, ATLAS, LAUDO_EXEMPLO, TERMINOLOGIA, DIRETRIZ

## Observations
- ✅ **NOVA ENTIDADE** — não existia no SKILL.md anterior
- ✅ Usa `@PrePersist` e `@PreUpdate` para timestamps
- ✅ Enum bem definido com 7 categorias médicas relevantes
- ✅ Nenhum dado LGPD (conhecimento genérico, não paciente-específico)
- ⚠️ Sem controller ou service — CRUD não implementado
- ⚠️ Repository existe mas sem custom queries — apenas CRUD padrão
- ⚠️ Sem integração com pgvector/embeddings — deveria alimentar o RAG store
- ⚠️ `tipoExameRelacionado` e `regiaoAnatomica` são String livres ao invés de enums

## Propósito no Pipeline
Esta entidade foi projetada para alimentar o sistema RAG do TILA:
1. Protocolos e terminologias são armazenados como `ConhecimentoMedico`
2. O conteúdo seria convertido em embeddings via LangChain4j
3. Esses embeddings seriam armazenados no pgvector
4. O ContentRetriever buscaria contexto relevante para geração de pré-laudos

**Status**: Entidade criada, mas pipeline de ingestão → embedding → store **NÃO IMPLEMENTADO**.

## Backlinks
- [[wiki/concepts/data-model]]
- [[context/ai-pipeline]]
