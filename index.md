# Tila_Brain Index
> Last updated: 2026-06-07 | Pages: 32 | Sources: 5 raw snapshots

## Concepts
| Page | Summary | Tags | Updated |
|---|---|---|---|
| [[wiki/concepts/data-model]] | Modelo ER completo, mapa de exposição LGPD, gaps de auditoria | data-model, lgpd, jpa | 2026-05-07 |
| [[wiki/concepts/backend-services]] | Inventário de services e repositories com violações encontradas | services, repositories | 2026-05-07 |
| [[wiki/concepts/api-endpoints]] | Todos os endpoints verificados no código com security gaps | api, endpoints, rest | 2026-05-07 |
| [[wiki/concepts/frontend-architecture]] | Inventário completo de componentes, rotas, services, e gaps | frontend, angular | 2026-05-07 |
| [[wiki/concepts/backend-patterns]] | Padrões reais: GenericResult, DTO records, service, audit log | backend, patterns | 2026-06-07 |
| [[wiki/concepts/angular-patterns]] | Padrões reais: Signals, standalone, inject(), interceptor, guard | frontend, patterns | 2026-06-07 |
| [[wiki/concepts/jwt-authentication]] | Implementação JWT: HMAC256, HttpOnly cookies, SecurityFilter | security, jwt | 2026-05-07 |
| [[wiki/concepts/laudo-medico]] | Definição e estrutura de laudos médicos brasileiros | medical, laudo | 2026-05-07 |
| [[wiki/concepts/dicom]] | Padrão DICOM: tags, scrubbing, gap atual no TILA | medical, dicom | 2026-05-07 |
| [[wiki/concepts/rag-vs-llm-wiki]] | Comparação RAG vs LLM Wiki (Karpathy) | architecture, rag | 2026-05-07 |
| [[wiki/concepts/laudo-patterns]] | Padrões estruturais de laudos: seções, linguagem, modalidades | medical, patterns | 2026-05-07 |
| [[wiki/concepts/langchain4j-multimodal-workaround]] | Padrão workaround p/ envio de texto + imagem sem @AiServices | ai, langchain4j, pattern | 2026-06-03 |
| [[wiki/concepts/state-sharing-reactivity]] | Compartilhamento de estado reativo em rotas com cache no frontend | frontend, state-management, signals | 2026-06-04 |
| [[wiki/concepts/backend-infrastructure-cache]] | Papel do Redis (in-memory cache) e MinIO (object storage) no backend | backend, cache, storage | 2026-06-04 |
| [[wiki/concepts/angular-state-management]] | Estratégias de gerência de estado e tráfego de dados entre telas no frontend | frontend, state-management | 2026-06-05 |
| [[wiki/concepts/redis-cache-patterns]] | Casos de uso de cache, sessions e rate limit no Tila | backend, cache | 2026-06-05 |
| [[wiki/concepts/minio-object-storage]] | Papel do MinIO para armazenamento de imagens e laudos na arquitetura | backend, storage | 2026-06-05 |

## Entities
| Page                                         | Summary                                                           | Tags                  | Updated    |
| -------------------------------------------- | ----------------------------------------------------------------- | --------------------- | ---------- |
| [[wiki/entities/spring-boot-backend]]        | Stack backend verificado: Spring Boot 4.0.3, Java 21, LangChain4j | backend, spring       | 2026-05-07 |
| [[wiki/entities/angular-frontend]]           | Stack frontend verificado: Angular 19.2.x, 14 componentes         | frontend, angular     | 2026-05-07 |
| [[wiki/entities/entity-usuario]]             | JPA entity: UUID, email, senha, UserDetails impl                  | jpa, entity, security | 2026-05-07 |
| [[wiki/entities/entity-medico]]              | JPA entity: nomeCompleto, crm, assinaturaDigital, 1:1 Usuario     | jpa, entity           | 2026-05-07 |
| [[wiki/entities/entity-paciente]]            | JPA entity: cpf, 4 campos LGPD, 1:N Exame                         | jpa, entity, lgpd     | 2026-05-07 |
| [[wiki/entities/entity-exame]]               | JPA entity: tipoExame, status, N:1 Paciente/Medico                | jpa, entity           | 2026-05-07 |
| [[wiki/entities/entity-laudo]]               | JPA entity: rascunhoIA, confidenceScore, StatusLaudo, @PrePersist | jpa, entity, ai       | 2026-05-07 |
| [[wiki/entities/entity-log-auditoria]]       | JPA entity: acao, dataHora, ipOrigem(null), 5 ações logadas       | jpa, entity, audit    | 2026-05-07 |
| [[wiki/entities/entity-conhecimento-medico]] | JPA entity: RAG knowledge base, 7 categorias médicas              | jpa, entity, ai       | 2026-05-07 |
| [[wiki/entities/entity-consulta]]            | Placeholder vazio — sem campos, sem tabela                        | jpa, placeholder      | 2026-05-07 |
| [[wiki/entities/spring-boot-4]]              | Spring Boot 4 framework reference page                            | framework, spring     | 2026-05-07 |

## Decisions (ADRs)
| Page | Summary | Tags | Updated |
|---|---|---|---|
| [[wiki/decisions/ADR-001-jwt-cookie-transport]] | HttpOnly cookie primário, Authorization header fallback | security, jwt | 2026-05-07 |
| [[wiki/decisions/ADR-002-api-response-pattern]] | GenericResult<T> envelope — 85% conforme, error path inconsistente | api, pattern | 2026-05-07 |
| [[wiki/decisions/ADR-003-security-architecture]] | 6 decisões tomadas, 8 pendentes (refresh token, encryption, etc.) | security, architecture | 2026-05-07 |
| [[wiki/decisions/ADR-004-langchain4j-multimodal]] | Uso de ChatModel direto ao invés de AiServices para imagens | ai, architecture | 2026-06-03 |
| [[wiki/decisions/ADR-005-redis-cache-strategy]] | Estratégia de caching, rate limit e session store com Redis | redis, cache, security | 2026-06-06 |
| [[wiki/decisions/ADR-006-minio-object-storage]] | Armazenamento de imagens médicas e laudos com MinIO e Presigned URLs | storage, s3, security | 2026-06-06 |
| [[wiki/decisions/ADR-007-angular-state-management]] | Padrão híbrido de rotas e cache com Angular Signals | angular, signals, state | 2026-06-06 |

## Sources
| Page | Summary | Tags | Updated |
|---|---|---|---|
| [[raw/codebase/snapshots/backend-structure]] | Snapshot completo do backend: deps, packages, config, security gaps | raw, snapshot | 2026-05-07 |
| [[raw/codebase/changelog/2026-05-16-ai-agent-foundation]] | Changelog: TilaRadiologistaAgent, system prompt RX Tórax, design LaudoService | raw, changelog, ai | 2026-05-16 |
| [[wiki/sources/2026-06-03-langchain4j-multimodal]] | Snapshot detalhando a resolução do conflito @UserMessage | source, ai | 2026-06-03 |
| [[wiki/sources/2026-06-03-integracao-laudo-ia]] | Fonte da conversa de desenvolvimento de Laudo IA e fixes críticos | source, ai | 2026-06-03 |
| [[wiki/sources/layout-state-management]] | Conversa de padronização de layout e infra (State Management, Redis, MinIO) | source, architecture | 2026-06-05 |
| [[wiki/sources/2026-06-07-refatoracao-auth-signals]] | Conversa sobre refatoração de DTOs, Repositories e uso de Signals | source, architecture | 2026-06-07 |

## Outputs
| Page | Summary | Tags | Updated |
|---|---|---|---|
| [[wiki/outputs/skill-md-update-2026-05-07]] | Comparação SKILL.md antigo vs novo: 19 confirmados, 13 desatualizados, 10 novos | output, skill | 2026-05-07 |

## Overview
| Page | Summary | Updated |
|---|---|---|
| [[wiki/overview]] | Síntese: TILA status, 6 critical gaps, 3 top priorities | 2026-05-07 |
