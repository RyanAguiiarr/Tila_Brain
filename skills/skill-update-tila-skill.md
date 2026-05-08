---
name: skill-update-tila-skill
trigger: "update tila skill" / "atualizar skill" / on feature complete
description: "Lê o estado atual dos repositórios TILA, compara com o SKILL.md do projeto e propõe atualizações."
---

# Skill: Update TILA Skill

## Context
O arquivo `SKILL.md` (ou equivalente) do projeto TILA deve refletir o estado real do codebase. Esta skill compara o código atual com a documentação e propõe atualizações precisas.

## Steps
1. Ler o estado atual de `c:\Projetos\Tila\Tila_BackEnd`:
   - Listar controllers e seus endpoints (`@RestController`, `@RequestMapping`)
   - Listar entidades JPA (`@Entity`)
   - Listar services e suas dependências
   - Verificar `SecurityFilterChain` config
   - Verificar `application.properties` / `application.yml`

2. Ler o estado atual de `c:\Projetos\Tila\Tila_Frontend`:
   - Listar componentes Angular e suas rotas
   - Listar services e seus endpoints consumidos
   - Verificar `app.routes.ts` ou equivalente
   - Verificar padrões de state management (Signals)

3. Comparar com o skill file existente do projeto (se houver) ou com `wiki/concepts/api-endpoints.md` e `wiki/concepts/angular-patterns.md`.

4. Propor atualizações específicas nas seções:
   - **§3 — Endpoints**: novos endpoints, endpoints removidos, mudanças de contract
   - **§4 — Frontend Routes**: novas rotas, componentes, fluxos
   - **§7 — Divergências**: padrões que divergem das convenções documentadas
   - **§9 — Sugestões de Melhoria**: issues identificados, refactors pendentes

5. Apresentar cada seção ao humano para aprovação individual.

6. Aplicar somente as seções aprovadas.

7. Atualizar `wiki/concepts/api-endpoints.md` e `wiki/concepts/angular-patterns.md` para espelhar as mudanças.

8. Append em `log.md`:
   ```
   ## [YYYY-MM-DD HH:MM] update-skill | TILA Skill Updated
   [Seções atualizadas: §3, §4, etc.]
   ```

## Rules
- NUNCA escrever no skill file sem aprovação explícita do humano para cada seção.
- Se encontrar divergências de convenção, reportar em §7 mas não corrigir automaticamente o código.
- Manter consistência entre o skill file e as páginas da wiki — ambos devem refletir a mesma realidade.
- Se o skill file do projeto não existir, criar um novo baseado no template de `context/coding-conventions.md`.
- Verificar se endpoints novos têm auth requirements documentados — se não, flaggar.
