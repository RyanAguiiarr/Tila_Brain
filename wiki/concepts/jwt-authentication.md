---
title: "JWT Authentication — TILA"
type: concept
tags: [security, jwt, authentication, spring-boot]
sources: []
last_updated: 2026-05-07
---

# JWT Authentication — TILA

## Visão Geral

O TILA utiliza **JSON Web Tokens (JWT)** para autenticação stateless. O token é gerado no login e enviado em todas as requests subsequentes. O backend valida o token sem necessidade de manter sessão no servidor.

## Implementação

### Algoritmo e Configuração
- **Algoritmo**: HMAC256 (chave simétrica compartilhada)
- **Expiração**: 1 hora
- **Payload claims**: `sub` (username/email), `roles` (array de roles), `iat` (issued at), `exp` (expiration)
- **Library**: `com.auth0:java-jwt` (via Spring Security)

### Transporte do Token
O TILA usa um modelo híbrido de transporte, decidido formalmente em [[wiki/decisions/ADR-001-jwt-cookie-transport]]:

1. **Primário — HttpOnly Cookie** (`jwt_token`)
   - Protegido contra XSS (JavaScript não pode ler)
   - Enviado automaticamente pelo browser
   - Requer `SameSite=Lax` e `Secure=true` em produção
   - CORS deve ter `allowCredentials: true`

2. **Fallback — Authorization Header** (`Bearer <token>`)
   - Para clientes mobile ou API consumers
   - Requer o client a gerenciar o armazenamento do token

### SecurityFilterChain

O filtro JWT é registrado **antes** do `UsernamePasswordAuthenticationFilter` na chain do Spring Security:

```java
http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);
```

### Fluxo de Autenticação

```
1. POST /auth/login { email, senha }
   │
   ├─ Validar credenciais (BCrypt)
   ├─ Gerar JWT com claims
   ├─ Setar cookie HttpOnly
   └─ Retornar GenericResult<LoginResponse>

2. GET /api/pacientes (authenticated)
   │
   ├─ JwtFilter extrai token do cookie/header
   ├─ Valida assinatura HMAC256
   ├─ Verifica expiração
   ├─ Carrega UserDetails do UsuarioRepository
   ├─ Seta SecurityContextHolder
   └─ Request segue para o controller

3. Token expirado
   │
   └─ JwtFilter não seta context → Spring retorna 401
```

### Configuração CORS (necessária para cookies)

Para que cookies HttpOnly funcionem cross-origin (frontend Angular em `localhost:4200`, backend em `localhost:8080`):

```java
config.setAllowCredentials(true);
config.setAllowedOrigins(List.of("http://localhost:4200"));
```

## Vulnerabilidades Conhecidas

1. **JWT Secret possivelmente hardcoded** — ver [[context/security-lgpd]] §Vulnerabilidades.
2. **Sem refresh token** — o usuário é forçado a re-autenticar a cada hora.
3. **Sem rate limiting** no endpoint `/auth/login` — vulnerável a brute force.

## Referências
- [[context/security-lgpd]] — Detalhes completos de segurança e LGPD
- [[wiki/entities/spring-boot-4]] — SecurityFilterChain pattern

## Backlinks
- [[wiki/overview]]
- [[wiki/decisions/ADR-001-jwt-cookie-transport]]
- [[context/security-lgpd]]
