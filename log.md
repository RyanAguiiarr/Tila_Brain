# Tila_Brain Log

---

## [2026-05-07 20:18] setup | Tila_Brain initial scaffold
Full directory structure created. CLAUDE.md written. 10 starter wiki pages created (7 concepts, 1 entity, 1 decision, 1 overview). 8 skills defined (ingest, query, lint, generate-laudo, review-exame, adr, capture-feature, update-tila-skill, dev-assistant + template). 4 context files written (project-identity, coding-conventions, security-lgpd, ai-pipeline, roadmap). Cron registry initialized with 7 crons. 2 scripts created (brain-boot, brain-sync).

## [2026-05-07 21:21] ingest | Initial codebase ingestion — TILA full audit
Performed full archaeological read of Tila_BackEnd and Tila_Frontend.
Backend: 7 entities, 7 endpoints, 5 services documented.
Frontend: 14 components, 10 routes documented.
Wiki pages created: 24.
ADRs recorded: ADR-001, ADR-002, ADR-003.
Security gaps found: 6 critical, 11 medium, 6 low.
Convention violations found: 14.
SKILL.md rewritten from scratch with verified data.
Key findings:
- Laudo entity EXISTS (contradicts previous SKILL.md)
- ConhecimentoMedico entity is NEW (undocumented)
- TilaRagConfig with LangChain4j 0.36.2 is NEW (undocumented)
- DB changed from TilaDB:5433 to vectorDB:5434
- 3 secrets hardcoded (JWT, DB password, Gemini API key)
- AI infrastructure 70% configured, 0% operational

## [2026-05-16 11:11] capture | Feature: AI Agent Foundation — Raio-X de Tórax
TilaRadiologistaAgent interface criada com @SystemMessage(fromResource) + @UserMessage parametrizado + Image multimodal.
System prompt Dr. TILA escrito — foco exclusivo RX de Tórax (PA e Perfil).
TilaRagConfig atualizado com bean tilaAgent() via AiServices.builder().
Decisão de escopo: agente analisa EXCLUSIVAMENTE Raio-X de Tórax na Fase 2.
Design completo documentado: ExameRepository (JOIN FETCH), 3 DTOs Laudo, LaudoService (4 métodos), LaudoController.
3 bugs críticos identificados e registrados no roadmap (API key hardcoded, txt no lugar errado, @V inválido).
Wiki atualizada: entity-laudo, api-endpoints, roadmap, project-identity, index.
Changelog criado: raw/codebase/changelog/2026-05-16-ai-agent-foundation.md

## [2026-06-02 19:42] fix | Correção de Bugs e Ajustes de Configuração
- Correção do bug de data no JPA callback `@PreUpdate` da entidade `Laudo`.
- Remoção da API Key hardcoded em `TilaRagConfig` substituindo por leitura via `${GEMINI_API_KEY}`.
- Atualização do status de progresso das pendências e bugs no `roadmap.md`.

## [2026-06-02 20:05] fix | Resolução de Conflito de Classpath do Jackson
- Alinhamento de dependências no [pom.xml](file:///c:/Projetos/Tila/Tila_BackEnd/tila/pom.xml) importando o `jackson-bom` na versão `2.18.2`.
- Isso resolve o conflito entre o Jackson 3 (Spring Boot 4) e o Jackson 2 (java-jwt/LangChain4j), fornecendo o enum `POJO` em `JsonFormat.Shape` e sanando o erro `java.lang.NoSuchFieldError`.

## [2026-06-03 21:10] ingest | Snapshot Multimodal LangChain4j
Ingestão de código documentando a solução do conflito da anotação `@UserMessage` com imagens multimodais no LangChain4j 1.0.1.
- Criada decisão [[ADR-004-langchain4j-multimodal]] formalizando o uso de `ChatModel` direto em vez de `AiServices`.
- Criado o conceito [[wiki/concepts/langchain4j-multimodal-workaround]] para documentar o padrão.
- Fonte mapeada em [[wiki/sources/2026-06-03-langchain4j-multimodal]].

## [2026-06-03 21:30] upgrade | skill-ingest.md v1.0 → v2.0
Reescrita completa da [[skills/skill-ingest]] (78 linhas → 500+ linhas).
- Adicionadas 3 variantes novas: Sessão de Debug, Conversa de Desenvolvimento, Dependência/Upgrade.
- Todas as variantes expandidas com exemplos reais do projeto (sessão 2026-06-03).
- Adicionados 6 diagramas Mermaid (fluxo de ingestão, sequência, cronologia de debug, timeline, mapa de referências, anatomia).
- Exemplos de código reais (antes/depois do LangChain4j multimodal).
- Pre-flight e Post-flight checklists expandidos com verificações granulares.
- Cross-references completos para todas as páginas do brain (skills, context, wiki, decisions).
- Seção de frontmatter obrigatório e regras de cross-referencing documentadas.

## [2026-06-03 21:38] upgrade | skill-dev-assistant.md v1.0 → v2.0
Reescrita completa da [[skills/skill-dev-assistant]] (57 linhas → 350+ linhas).
- Diagrama de arquitetura da skill (3 fases: ativação, desenvolvimento, pós).
- Tabela de decisão para leitura de contexto (o que ler dependendo da tarefa).
- Checklists granulares separados por camada: Backend (DI, API, DTOs, Optional, Segurança, Pipeline IA), Frontend (Componentes, Signals, Services, CSS), Database/JPA.
- Catálogo de erros conhecidos com 7 entradas reais (exceção, causa raiz, solução, referência).
- Exemplos de código antes/depois para cada padrão (correto vs anti-pattern).
- Hierarquia de prioridades documentada: Segurança > Convenções > Funcionalidade > Performance > Estilo.
- Regras de conflito e governança expandidas.

## [2026-06-03 21:38] upgrade | skill-update-tila-skill.md v1.0 → v2.0
Reescrita completa da [[skills/skill-update-tila-skill]] (52 linhas → 350+ linhas).
- Diagrama de comparação tri-way (código vs wiki vs SKILL.md).
- Procedimento de scan detalhado: 6 sub-steps para backend (controllers, entities, services, security, IA, Maven), 4 sub-steps para frontend (componentes, rotas, services, estado).
- Template completo do SKILL.md com 10 seções padronizadas.
- Tabela de divergências reais encontradas em 2026-06-03 (10 itens: versões, modelos, classes renomeadas).
- Diagrama de categorias de divergência (Novo, Removido, Alterado, Sincronizado).
- Tabela de sincronização wiki ↔ SKILL.md (qual seção atualiza qual página).

## [2026-06-03 21:51] ingest | Codebase Conversation (Integração IA e Bugs)
Ingestão de código vasculhando o `git status` e `git diff` do backend, utilizando a `skill-ingest.md` v2.0 para atualizar o Cérebro.
- Criada a conversa de desenvolvimento: [[raw/codebase/conversations/2026-06-03-integracao-laudo-ia]] contendo o timeline de todos os 6 principais eventos (Bug Fix JSON recursion, Bug Fix Auth NPE, Config Gemini v1beta, Feature POST /laudo, LaudoService, e refactor ChatModel).
- Atualizado o [[context/ai-pipeline]] para "Implementado (Orquestração)". Status saltou de 70% infraestrutural para 85% funcional com orquestração completa gerando `LaudoResponseDTO`. O diagrama de fluxo do sistema e a gap analysis foram substituídos por versões refletindo o workaround do `ChatModel` direto.
- Atualizados [[context/roadmap]] e [[context/security-lgpd]]: Marcados bugs críticos de vulnerabilidade NPE e de stack overflow como resolvidos.
- Atualizado [[wiki/concepts/api-endpoints]]: Inserido o endpoint real `POST /laudo` (LaudoController).
- Atualizado [[wiki/concepts/backend-services]]: Inserido `LaudoService` detalhando sua orquestração e refatorações no construtor e dependências.
- Fonte e referências cruzadas criadas em [[wiki/sources/2026-06-03-integracao-laudo-ia]] e mapeadas no [[index]].
