---
title: "AI Pipeline — TILA (Verificado)"
type: context
tags: [ai, pipeline, langchain4j, gemini, rag, pgvector]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# AI Pipeline — TILA

> Auditoria do estado real da implementação de IA em 2026-05-07.

---

## Status Geral: ⚠️ FUNDAÇÃO PARCIAL

A infraestrutura de IA foi **preparada** mas **não está conectada** ao fluxo de negócio. Existem entidades JPA, configuração de modelos, e embedding store, mas nenhum endpoint, service, ou agente que una tudo.

---

## O que EXISTE no código

### 1. Entidades JPA para IA
| Entidade | Status | Propósito |
|---|---|---|
| `Laudo` | ✅ Criada | Rascunho IA, texto final, achados JSON, impressão JSON, nota IA, confidence score, hash integridade |
| `ConhecimentoMedico` | ✅ Criada | Base de conhecimento médico para RAG (protocolos, anatomia, BI-RADS, etc.) |
| `StatusLaudo` | ✅ Enum | RASCUNHO → EM_REVISAO → ASSINADO / CANCELADO |
| `CategoriaConhecimento` | ✅ Enum | PROTOCOLO, ANATOMIA, ACR_BIRADS, ATLAS, LAUDO_EXEMPLO, TERMINOLOGIA, DIRETRIZ |

### 2. Configuração LangChain4j (`TilaRagConfig.java`)
| Bean | Implementação | Status |
|---|---|---|
| `ChatLanguageModel` | GoogleAiGeminiChatModel (gemini-2.5-flash) | ✅ Configurado |
| `EmbeddingModel` | GoogleAiEmbeddingModel (text-embedding-004) | ✅ Configurado |
| `EmbeddingStore<TextSegment>` | PgVectorEmbeddingStore (localhost:5433/vectorDB, tabela `tila_embeddings`, 768 dims) | ✅ Configurado |
| `ContentRetriever` | EmbeddingStoreContentRetriever (maxResults=7) | ✅ Configurado |

### 3. Dependências Maven
| Artefato | Versão | Status |
|---|---|---|
| langchain4j (core) | 0.36.2 (BOM) | ✅ No pom.xml |
| langchain4j-spring-boot-starter | via BOM | ✅ No pom.xml |
| langchain4j-google-ai-gemini | via BOM | ✅ No pom.xml |
| langchain4j-pgvector | via BOM | ✅ No pom.xml |

### 4. Configuração application.properties
- Gemini API key: via `${GEMINI_API_KEY}` (⚠️ + valor hardcoded abaixo)
- Chat model: gemini-1.5-flash (config) vs gemini-2.5-flash (Java code)
- Embedding model: text-embedding-004
- Temperature: 0.3
- Max output tokens: 4096
- Upload path: `./uploads/exames`
- Max upload: 50MB

### 5. Frontend (Mockado)
- `LaudoIaComponent` — tela de geração de laudo com dados mockados
- `CentroLaudosComponent` — tela de centro de laudos com dados mockados

---

## O que NÃO EXISTE

| Componente | Status | Descrição |
|---|---|---|
| TilaRadiologistaAgent (interface) | ❌ Não existe | Interface LangChain4j `@AiService` para o agente radiologista |
| System Prompt | ❌ Não existe | Instruções para o LLM sobre geração de laudos |
| LaudoService | ❌ Não existe | Service que orquestra Agent + Laudo entity |
| LaudoController | ❌ Não existe | Endpoints REST para laudos |
| Pipeline de ingestão | ❌ Não existe | Converter ConhecimentoMedico → embeddings → pgvector |
| DICOM reader | ❌ Não existe | Parser de arquivos DICOM |
| DICOM scrubber | ❌ Não existe | Remoção de metadados LGPD de DICOMs |
| CNN/Vision model | ❌ Não existe | Modelo de visão para análise de imagens |
| RLHF feedback | ❌ Não existe | Loop de feedback médico → modelo |
| Testes de IA | ❌ Não existe | Testes de qualidade do output |

---

## Divergência: Config vs Code

⚠️ O `application.properties` configura `gemini-1.5-flash`, mas o `TilaRagConfig.java` instancia `gemini-2.5-flash`. O código Java tem precedência pois cria o bean manualmente.

⚠️ O `EmbeddingStore` no código conecta em `localhost:5433` (port 5433), mas o `application.properties` configura o datasource em `localhost:5434`. São databases diferentes? Possível que vectorDB esteja em 5433 e TilaDB em 5434 (ou vice-versa).

---

## Pipeline Pretendido (6 estágios)

```
1. INGESTÃO          → Imagem DICOM + notas clínicas
                         ↓
2. SCRUBBING (LGPD)  → Remover metadados pessoais do DICOM    [❌ NÃO IMPLEMENTADO]
                         ↓
3. VISÃO (CNN)       → Extrair achados visuais em JSON          [❌ NÃO IMPLEMENTADO]
                         ↓
4. LLM + RAG         → Gerar pré-laudo em prosa técnica        [⚠️ INFRA PRONTA, SEM ORQUESTRAÇÃO]
                         ↓
5. HUMAN-IN-LOOP     → Médico revisa, edita, assina             [⚠️ ENTITY EXISTE, SEM TELA]
                         ↓
6. RLHF              → Feedback → melhora modelo futuro         [❌ NÃO IMPLEMENTADO]
```

---

## Bloqueios para Próximo Passo

1. **Criar `TilaRadiologistaAgent`** — interface `@AiService` com `@SystemMessage` definido
2. **Criar `LaudoService`** — orquestrar Agent → salvar Laudo → retornar DTO
3. **Criar `LaudoController`** — endpoints REST para gerar, listar, revisar laudos
4. **Pipeline de ingestão de conhecimento** — popular `ConhecimentoMedico` → converter em embeddings
5. **Resolver porta do pgvector** — confirmar se é 5433 ou 5434

## Referências
- [[wiki/entities/entity-laudo]]
- [[wiki/entities/entity-conhecimento-medico]]
- [[wiki/concepts/dicom]]
- [[wiki/concepts/laudo-patterns]]
- [[context/roadmap]]

## Backlinks
- [[wiki/overview]]
- [[context/roadmap]]
