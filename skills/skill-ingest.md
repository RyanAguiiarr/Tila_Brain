---
name: skill-ingest
trigger: "ingest this" / "injest this" / "/ingest"
description: "Ingere uma fonte nova (artigo, vídeo, snapshot de código, laudo) no raw/ e compila conhecimento na wiki/."
---

# Skill: Ingest

## Context
Quando o humano adiciona uma nova fonte de conhecimento — um artigo, transcrição de vídeo, snapshot de código, ou laudo anonimizado — esta skill processa a fonte, extrai os insights principais, e os materializa como páginas permanentes na wiki.

## Pre-flight Checklist
- [ ] Fonte está em `raw/` (ou é uma URL a ser baixada)?
- [ ] Tipo identificado: article | video | codebase | medical | laudo?
- [ ] Fonte contém dados sensíveis (PHI/CPF)? Se sim, ABORTAR e pedir anonimização.

## Steps

### Variante Padrão (Artigos / Material Geral)
1. Ler a fonte completamente.
2. Discutir com o humano 2–3 takeaways principais antes de escrever qualquer coisa.
3. Criar página de resumo em `wiki/sources/[YYYY-MM-DD]-[slug].md` com frontmatter completo.
4. Para cada conceito novo identificado:
   - Verificar se já existe uma página em `wiki/concepts/`.
   - Se sim: atualizar com as novas informações, citando a fonte.
   - Se não: criar nova página com frontmatter, conteúdo, e seção `## Backlinks`.
5. Para cada entidade nova (pessoa, ferramenta, biblioteca):
   - Mesma lógica em `wiki/entities/`.
6. Atualizar `wiki/overview.md` se a síntese geral muda significativamente.
7. Atualizar `index.md` com todas as páginas criadas/atualizadas.
8. Append em `log.md`:
   ```
   ## [YYYY-MM-DD HH:MM] ingest | [Título da Fonte]
   [Descrição breve do que foi adicionado e quais páginas wiki foram tocadas.]
   ```
9. Perguntar: "Desejo arquivar este resultado de ingest como uma síntese permanente?"

### Variante YouTube
1. Usar TranscriptAPI MCP para buscar a transcrição.
2. Salvar transcrição em `raw/videos/[YYYY-MM-DD]-[slug].md`.
3. Seguir passos 1–9 da variante padrão.

### Variante Codebase Snapshot
1. Ler o diff ou arquivo de `raw/codebase/snapshots/`.
2. Identificar decisões arquiteturais relevantes.
3. Criar ADR usando `skills/skill-adr.md` se necessário.
4. Atualizar `wiki/concepts/api-endpoints.md`, `wiki/concepts/angular-patterns.md`, ou outras páginas técnicas conforme necessário.
5. Seguir passos 6–9 da variante padrão.

### Variante Laudo Anonimizado
1. Verificar anonimização (ZERO dados de paciente).
2. Extrair padrões estruturais: seções, estilo de linguagem, terminologia.
3. Atualizar `wiki/concepts/laudo-patterns.md` com os novos padrões.
4. NUNCA copiar conteúdo clínico — apenas estrutura e padrões.
5. Seguir passos 6–9 da variante padrão.

## Output format
- 1 página em `wiki/sources/`
- 0–15 páginas criadas/atualizadas em `wiki/concepts/` e `wiki/entities/`
- `index.md` atualizado
- `log.md` com nova entrada
- `wiki/overview.md` atualizado (se necessário)

## Rules
- NUNCA modificar arquivos em `raw/` — são imutáveis.
- NUNCA incluir dados reais de pacientes.
- Toda página wiki DEVE ter frontmatter e seção `## Backlinks`.
- Máximo de 15 páginas criadas/atualizadas por ingest para manter qualidade.
- Quando um claim médico não pode ser verificado, marcar com: `> ⚠️ Não verificado — fonte necessária.`
- Cross-references devem usar `[[wiki/path/page]]` syntax.

## Post-flight Checklist
- [ ] Todas as páginas criadas possuem frontmatter completo?
- [ ] Todas as páginas possuem seção `## Backlinks`?
- [ ] `index.md` reflete todas as mudanças?
- [ ] `log.md` foi atualizado?
- [ ] Nenhum dado sensível foi incluído?
