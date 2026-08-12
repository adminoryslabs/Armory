# Design: MCP Catalog Section

## Technical Approach

Pure content-structure change — no code, build, or runtime involved. Add a third
content convention (`mcps/<name>.md`, single-file, mirrors `prompts/<name>.md`)
per the external contract at `contracts/mcp-catalog-section.md`. Two entries in
scope: `mcps/github.md` and `mcps/obsidian.md`. README.md/CLAUDE.md edits are a
separate, gated concern (literal string replacement, blocked on out-of-band repo
rename confirmation) — not part of the `mcps/` convention itself.

## Architecture Decisions

| Decision | Choice | Alternative rejected | Rationale |
|---|---|---|---|
| Body language | Spanish, prose paragraphs, no headers | English (SDD artifact default) | Existing `prompts/*.md` (incl. `ejecutar-multirepo.md`) are all Spanish, no-header prose addressed directly to the agent — this is content extending that convention, not an SDD artifact |
| `docs_url` on `github.md` | Omit the key entirely unless apply-phase finds a real, distinct docs URL | Fill with README URL as a stand-in | Contract marks `docs_url` optional/nullable; proposal forbids fabricated values — a fake docs_url is worse than a missing one |
| README/CLAUDE.md rename edits | Hard blocking yes/no question to the user before touching either file | Auto-detect via `git remote -v` or GitHub API | `git remote -v` is unreliable (GitHub redirects renamed repos silently, git doesn't auto-update the URL) and the proposal explicitly requires *explicit user confirmation*, not inference |
| MCPs section in README.md | Do not add one | Document the new `mcps/` folder in README while editing it for the rename | Explicitly out of scope per proposal ("Fixing README's stale skill list beyond lines also touched by the rename update") |

## File Changes

| File | Action | Description |
|------|--------|--------------|
| `mcps/github.md` | Create | GitHub MCP entry: frontmatter + hand-written install prompt |
| `mcps/obsidian.md` | Create | Obsidian MCP entry (plugin-based, not npx) + install prompt with 3-item stop-and-ask gate |
| `README.md` | Modify (gated) | Replace literal `adminoryslabs/Skills` → `adminoryslabs/Armory`, 7 occurrences: line 7, lines 13–17, line 23 — only if rename confirmed |
| `CLAUDE.md` | Modify (gated) | Replace literal `adminoryslabs/Skills` → `adminoryslabs/Armory` (line 3 as `github.com/adminoryslabs/Skills`), plus lines 13, 33, 36 — only if rename confirmed |

## Interfaces / Contracts

### Frontmatter templates (per external contract)

```yaml
# mcps/github.md
---
name: github
description: <short, e.g. "MCP oficial de GitHub — issues, PRs, repos y búsqueda desde el agente">
type: mcp
tags: [github, git, mcp]
repo_url: https://github.com/github/github-mcp-server
# docs_url: omit unless a real, distinct docs site is confirmed at apply time
---
```

```yaml
# mcps/obsidian.md
---
name: obsidian
description: <short, e.g. "API REST local de Obsidian para que el agente lea/escriba notas del vault">
type: mcp
tags: [obsidian, notes, mcp, plugin]
repo_url: https://github.com/coddingtonbear/obsidian-local-rest-api
docs_url: https://coddingtonbear.github.io/obsidian-local-rest-api/
---
```

### Shared install-prompt body template

Both bodies follow the same 5-paragraph skeleton (prose, no markdown headers,
same register as `prompts/ejecutar-multirepo.md`):

1. **Contexto** — qué es el MCP y qué habilita para el agente (1–2 frases).
2. **Verificación previa** — instruye al agente a revisar el método de
   instalación vigente en `repo_url`/`docs_url` en vez de asumir el de este
   prompt, porque los MCP servers cambian de forma de instalación seguido.
3. **Prerrequisitos — detenerse y preguntar** (bulleted, MCP-specific):
   - `github.md`: token de acceso (scopes según el repo oficial); si el
     usuario no lo dio, PARAR y pedirlo — usar `<TU_TOKEN_AQUI>`, nunca un
     valor de ejemplo real.
   - `obsidian.md`: confirmar los 3 ítems antes de seguir — (a) plugin
     `Local REST API` instalado y habilitado dentro de Obsidian, (b) bearer
     token generado desde la pestaña de configuración del plugin, (c) HTTP
     (puerto 27123, requiere habilitarlo) vs HTTPS (puerto 27124, default,
     certificado autofirmado). Ninguno de los tres se asume — si falta uno,
     PARAR y preguntar.
4. **Instalación/configuración** — pasos concretos para el config del cliente
   MCP, con placeholders explícitos; instruye adaptar comando/claves exactas
   a lo que digan los docs oficiales en el momento.
5. **Verificación final** — cómo confirmar que quedó funcionando (ej. una
   tool call esperable, o un smoke test mínimo).

## Gating Logic for README.md / CLAUDE.md (sdd-apply MUST follow)

1. Before touching either file, ask the user explicitly: *"¿Confirmás que el
   rename de `adminoryslabs/Skills` → `adminoryslabs/Armory` ya se hizo en
   GitHub?"* — blocking, yes/no only.
2. **If not confirmed** → skip both files entirely. Leave a note in the apply
   summary: "README.md/CLAUDE.md rename edits pending — apply again after
   confirming the GitHub rename." Do not touch either file.
3. **If confirmed** → replace only the literal string `adminoryslabs/Skills`
   with `adminoryslabs/Armory` at the exact lines listed in File Changes
   above. Do not touch unrelated stale content in the same files (e.g.
   README's missing 6th skill) — out of scope.
4. Verify with a literal search for `adminoryslabs/Skills` across both files
   after editing — it must return zero matches.

## Testing / Validation Strategy

| Check | What | How |
|---|---|---|
| Frontmatter | Valid YAML, matches contract shape exactly | Manual read-back against `contracts/mcp-catalog-section.md` |
| No fabricated values | `repo_url`/`docs_url` real or omitted, no placeholder repo values | Manual review |
| Install prompt gating | Both bodies include explicit stop-and-ask instructions (github: token; obsidian: 3 items) | Manual review against template above |
| Rename gate | README/CLAUDE.md untouched unless user confirmed rename | Manual — apply phase reports which branch it took |
| Post-edit literal check | Zero remaining `adminoryslabs/Skills` occurrences if gate passed | Grep after edit |

## Migration / Rollout

No migration required. All changes are additive Markdown files plus two gated
literal-string edits. Rollback via `git revert` of the commit.

## Open Questions

- [ ] Does `github-mcp-server` have a distinct `docs_url` separate from its
      README? Verify at apply time; omit the key if none is found.
- [ ] Has the `adminoryslabs/Skills` → `adminoryslabs/Armory` GitHub rename
      actually happened yet? Blocks README/CLAUDE.md edits until the user
      confirms — resolved via the gating logic above, not guessed here.
