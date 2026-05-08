---
title: "ADR-003: Security Architecture"
type: decision
tags: [architecture, adr, security, jwt, bcrypt]
sources: [raw/codebase/snapshots/backend-structure.md]
last_updated: 2026-05-07
---

# ADR-003: Arquitetura de Segurança

## Status
Partially Implemented

## Context
O TILA trata dados médicos sensíveis protegidos pela LGPD (Art. 11 — dados de saúde). A arquitetura de segurança precisa proteger autenticação, autorização, transporte, e armazenamento.

## Decisões Tomadas

### 1. JWT com HttpOnly Cookie (ADR-001)
- Transporte primário via cookie `accessToken` (HttpOnly, SameSite:Lax)
- Fallback via header `Authorization: Bearer`
- **Status**: ✅ Implementado

### 2. BCrypt para Senhas
- BCryptPasswordEncoder com strength padrão (10 rounds)
- **Status**: ✅ Implementado

### 3. Sessions STATELESS
- `SessionCreationPolicy.STATELESS` — sem estado no servidor
- **Status**: ✅ Implementado

### 4. HMAC256 para JWT
- Secret compartilhado (simétrico) com auth0/java-jwt
- **Status**: ✅ Implementado (⚠️ secret hardcoded)

### 5. CSRF Desabilitado
- Justificativa: API REST stateless com JWT não requer CSRF
- HttpOnly cookie com SameSite:Lax mitiga parcialmente
- **Status**: ✅ Implementado

### 6. CORS Restrito
- Apenas `http://localhost:4200` permitido
- **Status**: ✅ Implementado (apenas dev)

### 7. Role-Based Access Control (RBAC)
- 3 roles: MEDICO, PACIENTE, ADMIN
- ADMIN herda MEDICO (implementado inline em `getAuthorities()`)
- **Status**: ⚠️ Parcial (hierarquia inline, sem RoleHierarchy bean)

## Decisões NÃO Tomadas (Pendentes)

| Decisão | Impacto | Prioridade |
|---|---|---|
| Refresh Token | UX — re-login a cada hora | 🟡 MÉDIA |
| Token Revocation (blacklist) | Segurança — logout efetivo | 🟡 MÉDIA |
| 2FA (TOTP/SMS) | Segurança — proteção contra credentials leak | 🔵 BAIXA |
| Rate Limiting | Segurança — proteção contra brute force | 🟡 MÉDIA |
| RSA/RS256 ao invés de HMAC | Segurança — key rotation mais fácil | 🔵 BAIXA |
| OAuth2/OIDC | Escalabilidade — SSO, providers externos | 🔵 BAIXA |
| Criptografia em repouso (AES-256) | LGPD — dados sensíveis no DB | 🔴 ALTA |
| Secure=true no cookie | Segurança — JWT não trafega via HTTP | 🔴 ALTA (em prod) |

## Consequences

### Positivas
- Autenticação funcional de ponta a ponta
- Senhas nunca armazenadas em texto plano
- Cookie HttpOnly previne XSS sobre token
- Stateless permite escalar horizontalmente

### Negativas
- 3 secrets hardcoded em application.properties
- Sem refresh token — UX degradada
- Sem rate limiting — brute force possível
- Sem criptografia em repouso — LGPD não conforme

## Backlinks
- [[wiki/decisions/ADR-001-jwt-cookie-transport]]
- [[context/security-lgpd]]
- [[wiki/concepts/api-endpoints]]
