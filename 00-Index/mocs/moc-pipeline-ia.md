---
title: "MOC — Pipeline de IA"
type: moc
cluster: pipeline-ia
---

# MOC — Pipeline de IA

> Mapa de navegação para notas relacionadas ao pipeline de inteligência artificial.

## Decisões
- [[negocio/permanent/decisoes/ADR-005-langchain4j-gemini-stack-ia]] — Escolha de LangChain4j + Gemini
- [[negocio/permanent/decisoes/ADR-006-pgvector-postgresql-embeddings]] — pgvector para embeddings

## Snapshots
- [[codebase/snapshots/backend-ai-agent-2026-06-09]] — Estado real do pipeline: 85% orquestração, 30% pipeline completo

## Domínio
- [[negocio/permanent/produto/tila-resolve-gargalo-de-laudos-manuais]] — Proposta de valor do produto
- [[negocio/permanent/medico/laudo-ia-exige-revisao-humana-obrigatoria]] — Por que IA gera rascunhos

## Estado atual (2026-06-09)
| Componente | Status |
|---|---|
| LangChain4j integration | ✅ Funcional |
| ChatModel (Gemini) | ✅ Funcional |
| LaudoService orquestração | ✅ Funcional |
| EmbeddingModel | ✅ Configurado |
| pgvector EmbeddingStore | ✅ Configurado |
| ContentRetriever (RAG) | ⚠️ Configurado mas desconectado |
| TilaRadiologistaAgent (AiServices) | ⚠️ Bean criado mas não usado |
| DICOM pipeline | ❌ Não implementado |
| CNN pré-processamento | ❌ Não implementado |
| RLHF feedback loop | ❌ Não implementado |
| RAG ingestão de dados | ❌ Não implementado |
