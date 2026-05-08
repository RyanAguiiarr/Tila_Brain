---
name: skill-dev-assistant
trigger: "activates when working on TILA code"
description: "Assistente de desenvolvimento que garante que todo código TILA respeite as convenções, segurança e decisões arquiteturais documentadas."
---

# Skill: Dev Assistant

## Context
Esta skill ativa automaticamente quando o agente está trabalhando em código dos repositórios TILA. Ela funciona como uma camada de governança que previne desvios de convenção e garante que o conhecimento da wiki seja aplicado.

## On Activation (before writing any code)
1. Ler `context/coding-conventions.md` — as convenções vigentes.
2. Ler `context/security-lgpd.md` — requisitos de segurança.
3. Ler `wiki/decisions/` — ADRs relevantes ao componente sendo modificado.
4. Ler `wiki/concepts/api-endpoints.md` se trabalhando no backend.
5. Ler `wiki/concepts/angular-patterns.md` se trabalhando no frontend.

## During Development

### Backend (Spring Boot 4 / Java 21)
Antes de sugerir qualquer código, verificar:
- [ ] API responses usam `ResponseEntity<GenericResult<T>>`?
- [ ] DTOs são Java records com Bean Validation?
- [ ] Services usam constructor injection (sem `@Autowired`)?
- [ ] Nenhum secret hardcoded (usar env vars)?
- [ ] Novo endpoint tem `@PreAuthorize` ou check de role?
- [ ] Entidade JPA segue naming conventions existentes?
- [ ] Pagination é server-side (não carregar toda a tabela)?
- [ ] Optional handling: `.orElseThrow()` ao invés de `.get()`?

### Frontend (Angular 19)
Antes de sugerir qualquer código, verificar:
- [ ] Componente é standalone?
- [ ] State management usa Angular Signals (`signal`, `computed`, `effect`)?
- [ ] CSS é vanilla (sem Tailwind)?
- [ ] Service usa `firstValueFrom()` para conversão a Promise quando necessário?
- [ ] Template usa modern control flow (`@if`, `@for`, `@switch`)?
- [ ] Forms usam `[(ngModel)]` ou ReactiveFormsModule consistentemente?
- [ ] Rotas estão registradas em `app.routes.ts`?

## After Feature Completion
1. Executar checklist de verificação final:
   - [ ] Código compila sem erros?
   - [ ] Convenções foram respeitadas?
   - [ ] Nenhuma vulnerabilidade de segurança introduzida?
2. Acionar `skill-capture-feature.md` para capturar a feature na wiki.
3. Acionar `skill-update-tila-skill.md` para atualizar o skill file do projeto.

## Rules
- Se o humano pedir algo que contradiz uma ADR, ALERTAR antes de prosseguir.
- Se o humano pedir algo que viola LGPD, RECUSAR e explicar o porquê.
- Se uma nova convenção é introduzida, perguntar: "Isto deve ser registrado como uma nova convenção em `context/coding-conventions.md`?"
- Sempre justificar desvios de convenção com referência à ADR ou decisão explícita.
- Se encontrar bugs ou code smells durante o desenvolvimento, reportar proativamente.
- Prioridade: Segurança > Convenções > Funcionalidade > Performance > Estilo.
