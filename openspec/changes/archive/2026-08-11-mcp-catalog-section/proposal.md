# Proposal: MCP Catalog Section

## Intent

The catalog currently indexes two content types (`skills/`, `prompts/`) but has no
first-class way to list MCP servers users actually connect to their agents (GitHub,
Obsidian, etc.). Users manually track install/setup steps for MCPs outside Armory.
This change adds a third content convention (`mcps/<name>.md`) so MCP entries can be
discovered and installed the same way skills/prompts are — closing a real gap in
catalog completeness, not a hypothetical one (contract already confirms `registry-api`,
`registry-web`, and `launcher-desktop` are ready to consume `type: mcp`).

## Scope

### In Scope
- New `mcps/<name>.md` convention (single-file, mirrors `prompts/`), per the existing
  frontmatter contract at `D:/DMC_Courses/Armory/contracts/mcp-catalog-section.md`
  (`name`, `description`, `type: mcp`, `tags`, `repo_url`, `docs_url`).
- `mcps/github.md`: `repo_url: https://github.com/github/github-mcp-server` (official,
  confirmed), hand-written install prompt.
- `mcps/obsidian.md`: `repo_url: https://github.com/coddingtonbear/obsidian-local-rest-api`,
  `docs_url: https://coddingtonbear.github.io/obsidian-local-rest-api/`. Install prompt
  must clarify this is an Obsidian plugin (Community Plugins or BRAT), not an npx/npm
  process, and must instruct the agent to stop and ask the user for: plugin
  enabled-confirmation, bearer token from plugin settings, and HTTP vs HTTPS transport
  choice — never invent these.
- README.md / CLAUDE.md literal `adminoryslabs/Skills` reference updates — gated: only
  executed after the user confirms the GitHub repo rename to `adminoryslabs/Armory` has
  actually happened.

### Out of Scope
- The GitHub repo rename itself (manual GitHub Settings action, out-of-band).
- `registry-api`/`registry-web`/`launcher-desktop` changes (separate repos, separate SDD cycles).
- Backfilling the missing `type: skill` field on the 5 skills that omit it (pre-existing, unrelated).
- Fixing README's stale skill list beyond lines also touched by the rename update.

## Capabilities

### New Capabilities
- `mcp-catalog-content`: defines and populates the `mcps/<name>.md` convention (frontmatter + hand-written install prompt body) for MCP entries.

### Modified Capabilities
- None.

## Approach

1. Create `mcps/` folder with `github.md` and `obsidian.md`, each conforming exactly to
   the existing external contract (already validated, no redefinition needed here).
2. Write install prompts by hand per each project's official README/plugin docs; use
   explicit placeholders (`<TU_TOKEN_AQUI>`) for credentials, never fabricated examples.
3. Defer README/CLAUDE.md literal-reference updates behind an explicit user-confirmation
   gate on the rename — do not attempt the rename itself.

## Affected Areas

| Area | Impact | Description |
|------|--------|--------------|
| `mcps/github.md` | New | GitHub MCP entry, install prompt |
| `mcps/obsidian.md` | New | Obsidian MCP entry, install prompt |
| `README.md` | Modified (gated) | Update `adminoryslabs/Skills` refs only after rename confirmed |
| `CLAUDE.md` | Modified (gated) | Same gate as above |

## Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Rename not yet done when this change is applied | Medium | Gate README/CLAUDE.md edits behind explicit user confirmation; skip if unconfirmed |
| Obsidian install prompt assumes wrong transport/token state | Low | Prompt instructs agent to stop and ask user, never assume |

## Rollback Plan

All changes are additive Markdown files plus two gated literal-string edits. Revert via
`git revert` of the commit; no data migration or external state involved.

## Dependencies

- `contracts/mcp-catalog-section.md` (sibling repo, already exists — read-only dependency).
- User confirmation of the GitHub rename before touching README.md/CLAUDE.md.

## Success Criteria

- [ ] `mcps/github.md` and `mcps/obsidian.md` exist, valid frontmatter, real URLs, no placeholder repo/docs values.
- [ ] Both install prompts explicitly gate on missing external setup rather than assuming it.
- [ ] README/CLAUDE.md rename edits either applied (if confirmed) or explicitly left pending with a note.
