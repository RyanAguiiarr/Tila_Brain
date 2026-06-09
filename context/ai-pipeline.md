---
title: "TILA — AI Pipeline"
type: context
last_updated: 2026-06-09
---

# TILA — AI Pipeline
> Honest status based on real code audit of 2026-06-09.
> See full details: [[codebase/snapshots/backend-ai-agent-2026-06-09]]

## Current architecture

```
[Imagem JPEG/PNG] → [LaudoService] → [Gemini 2.5-flash via ChatModel] → [GeminiLaudoResponse] → [Laudo entity]
```

## What WORKS (verified in code)

| Component | Status | File |
|---|---|---|
| ChatModel bean (Gemini) | ✅ Functional | TilaRagConfig.java |
| Multimodal message (text+image) | ✅ Functional | LaudoService.java |
| System prompt loading | ✅ Configured | classpath:prompts/radiologista-system.txt |
| JSON response parsing | ✅ Functional | LaudoService.parseRespostaIA() |
| Rascunho formatting | ✅ Functional | LaudoService.formatarRascunhoTexto() |
| Laudo persistence | ✅ Functional | LaudoRepository |

## What is CONFIGURED but DISCONNECTED

| Component | Status | Issue |
|---|---|---|
| TilaRadiologistaAgent interface | ⚠️ Dead code | LaudoService uses ChatModel directly, not AiServices |
| EmbeddingModel (gemini-embedding-001) | ⚠️ Unused | No ingest pipeline to create embeddings |
| EmbeddingStore (pgvector) | ⚠️ Empty | No data in tila_embeddings table |
| ContentRetriever (RAG) | ⚠️ Disconnected | Wired to tilaAgent bean but tilaAgent is never called |

## What DOES NOT EXIST

| Component | Status |
|---|---|
| DICOM pipeline | ❌ Not started |
| CNN preprocessing | ❌ Not started |
| RLHF feedback capture | ❌ Not started |
| RAG data ingestão | ❌ Not started |
| Multi-modality support | ❌ Only RX Tórax |
| IA monitoring/metrics | ❌ Not started |

## Known conflict
- `TilaRagConfig.java` creates ChatModel with `gemini-2.5-flash`
- `application.properties` configures via auto-config with `gemini-1.5-flash`
- Both produce a ChatModel bean — potential conflict depending on bean priority
