---
title: "Segurança e LGPD — TILA (Verificado)"
type: context
tags: [security, lgpd, jwt, audit, compliance]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# Segurança e LGPD — TILA

> Auditoria de segurança completa baseada no código real em 2026-05-07.

---

## Implementação de Segurança

### JWT
- **Algoritmo**: HMAC256 (auth0/java-jwt 4.4.0)
- **Issuer**: "TILA-APP"
- **Subject**: email do usuário
- **Claims adicionais**: `role` (perfil do usuário)
- **Expiry**: 1 hora (fixo, offset -03:00)
- **Secret**: `@Value("${api.security.token.secret}")` → 🔴 **HARDCODED** `"Cucamole@123"` no .properties
- **Refresh token**: ❌ NÃO IMPLEMENTADO

### Cookie Transport
- **Nome**: `accessToken`
- **HttpOnly**: ✅ true
- **Secure**: ❌ false (mesmo em produção)
- **SameSite**: Lax
- **Max-Age**: 3600 (1h)
- **Path**: /

### JWT Filter (SecurityFilter)
- Extrai token: cookie `accessToken` → fallback header `Authorization: Bearer`
- Valida via `TokenService.getSubject()`
- Busca usuário via `usuarioRepository.findByEmail(subject).get()` → 🔴 **NPE se usuário deletado**
- Seta `SecurityContextHolder` com authorities do usuário

### Password Encoding
- **Encoder**: BCryptPasswordEncoder (default strength = 10)
- ✅ Corretamente configurado como `@Bean`

### Session Management
- ✅ STATELESS (`SessionCreationPolicy.STATELESS`)

### CORS
- **Allowed Origins**: `http://localhost:4200` (apenas)
- **Allowed Methods**: GET, POST, PUT, DELETE, OPTIONS
- **Allowed Headers**: `*` (⚠️ wildcard — aceitável em dev, não em prod)
- **Allow Credentials**: ✅ true (necessário para cookies)
- ⚠️ Sem configuração para produção (nenhum profile configurado)

### CSRF
- ❌ **DESABILITADO** (`csrf.disable()`)
- Justificativa: API stateless com JWT → CSRF menos relevante
- ⚠️ Com HttpOnly cookie, CSRF é parcialmente mitigado por SameSite:Lax

### Authorization Rules (SecurityFilterChain)
| Pattern | Auth Requirement |
|---|---|
| OPTIONS /** | Público |
| /auth/** | Público |
| /medicos/** | ROLE_MEDICO |
| /paciente/** | ROLE_MEDICO ou ROLE_PACIENTE |
| Qualquer outro | Autenticado |

⚠️ `GET /logs` cai em "qualquer outro" → qualquer autenticado vê os logs → deveria ser ADMIN only.

---

## LGPD — Status de Conformidade

### Implementado
| Requisito | Status | Detalhes |
|---|---|---|
| Autenticação JWT | ✅ | HttpOnly cookie + BCrypt |
| Roles/Perfis | ✅ | MEDICO, PACIENTE, ADMIN |
| Logs de Auditoria | ⚠️ Parcial | Entity existe, 5 ações logadas, ipOrigem **sempre null** |
| Senhas hash | ✅ | BCrypt |

### NÃO Implementado
| Requisito | Status | Prioridade |
|---|---|---|
| Criptografia em repouso (AES-256) | ❌ | 🔴 ALTA |
| Anonimização de dados pessoais | ❌ | 🔴 ALTA |
| DICOM scrubbing (metadados) | ❌ | 🟡 MÉDIA |
| OCR em imagens (nomes em ultrassom) | ❌ | 🟡 MÉDIA |
| Consentimento explícito do paciente | ❌ | 🔴 ALTA |
| Direito de acesso (DSAR) | ❌ | 🟡 MÉDIA |
| Direito de retificação | ⚠️ Parcial | Sem PUT /paciente endpoint |
| Direito de exclusão | ❌ | Sem DELETE /paciente endpoint |
| Vault de identidade (UUID isolados) | ❌ | 🔵 BAIXA |
| Audit completo (CRUD operations) | ⚠️ Parcial | Apenas CREATE e READ logados |
| IP nos logs | ⚠️ Parcial | Campo existe, nunca populado |
| Rate limiting | ❌ | 🟡 MÉDIA |
| 2FA | ❌ | 🔵 BAIXA |

---

## Vulnerabilidades Encontradas

### 🔴 CRITICAL
1. **JWT secret hardcoded**: `"Cucamole@123"` em `application.properties` — mover para `${JWT_SECRET}` env var
2. **DB password hardcoded**: `Cucamole@123` em `application.properties` — mover para env var
3. **Gemini API key hardcoded**: `AIzaSyBkM8J29x9...` em `application.properties` — mover para env var (duplicado: config via `${GEMINI_API_KEY}` existe mas valor hardcoded abaixo)
4. **`medico.get()` sem verificação**: em `AutenticacaoController.login()` e `obterDadosLogado()` — NPE crash se médico não encontrado
5. **`findByEmail(subject).get()` no SecurityFilter**: NPE se usuário deletado mas token válido
6. **13 campos LGPD sem proteção**: CPF, nome, telefone, dados médicos armazenados em texto plano

### 🟡 MEDIUM
7. **GET /logs sem restrição de role**: qualquer autenticado vê todos os logs
8. **CORS wildcard headers**: `*` deve ser restrito em produção
9. **ipOrigem nunca populado**: campo de auditoria sempre null
10. **Sem paginação**: /paciente e /logs retornam tudo — DoS potencial com muitos registros
11. **ddl-auto=update**: perigoso em produção — pode alterar schema acidentalmente
12. **Secure=false no cookie**: JWT trafega sem TLS
13. **Sem Flyway/Liquibase**: sem versionamento de schema
14. **PacienteResponseDTO retorna entity**: expõe dados internos do JPA

### 🔵 LOW
15. **Sem rate limiting**: endpoints públicos (/auth/*) vulneráveis a brute force
16. **Sem OpenAPI/Swagger**: API não documentada formalmente
17. **Sem testes de segurança**: nenhum teste unitário/integração para segurança

---

## Correções Recomendadas (Prioridade)

1. 🔴 Mover TODOS os secrets para variáveis de ambiente
2. 🔴 Substituir `.get()` por `.orElseThrow()` em todos os Optional
3. 🔴 Restringir `GET /logs` para `ROLE_ADMIN` only
4. 🟡 Implementar paginação server-side em todos os endpoints de listagem
5. 🟡 Popular `ipOrigem` via `HttpServletRequest.getRemoteAddr()`
6. 🟡 Reescrever `GlobalExceptionHandler` para retornar `GenericResult.error()`
7. 🟡 Criar environment profiles (dev, prod) com configs separadas
8. 🟡 Implementar PUT e DELETE em /paciente (LGPD direito de retificação/exclusão)
9. 🔵 Adicionar rate limiting com Spring Boot Rate Limiter ou Resilience4j
10. 🔵 Adicionar Flyway para versionamento de schema

## Referências
- [[wiki/decisions/ADR-001-jwt-cookie-transport]]
- [[wiki/decisions/ADR-003-security-architecture]]
- [[wiki/concepts/data-model]]
- [[wiki/concepts/api-endpoints]]

## Backlinks
- [[wiki/overview]]
- [[context/roadmap]]
