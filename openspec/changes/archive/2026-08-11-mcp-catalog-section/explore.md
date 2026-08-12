# Exploration: Sección de MCPs en el catálogo Armory (`mcp-catalog-section`)

## Current State

`adminoryslabs-skills` is pure Markdown content, two existing conventions:

- **`skills/<name>/SKILL.md`** (6 folders: `generar-backlog`, `generar-prd`, `generar-prd-conversacional`, `generar-tech-design`, `revision-adversarial`, `descomponer-multirepo`) — folder can carry `assets/`/`references/`.
- **`prompts/<name>.md`** (3 files: `generar-claude-md-padre.md`, `generar-claude-md-hijo.md`, `ejecutar-multirepo.md`) — single file, no assets.

Frontmatter compliance check (read actual files, not assumed):
- `type` field is **inconsistently applied on skills**: only `skills/descomponer-multirepo/SKILL.md` declares `type: skill`. The other 5 skills omit `type` entirely (`generar-prd`, `generar-tech-design`, `revision-adversarial`, `generar-backlog`, `generar-prd-conversacional`).
- All 3 prompts **do** declare `type: prompt` explicitly and consistently — this convention is actually followed on the `prompts/` side.
- This confirms the gotcha already documented in the repo's own CLAUDE.md, with exact evidence now attached.

`README.md` documents install via `npx skills add adminoryslabs/Skills --skill <name>` and lists all 5 skills present before `descomponer-multirepo` was added — it's already stale by one skill (a pre-existing, unrelated gap, also documented in CLAUDE.md).

No `mcps/` folder exists yet. No `contracts/` folder exists anywhere in this repo. No `openspec/` folder existed before this change — this is the first OpenSpec-mode SDD change here.

## Affected Areas

- `mcps/` (new folder, does not exist) — target location for `obsidian.md`, `github.md`.
- `CLAUDE.md:3,13,33,36` — literal `adminoryslabs/Skills` references (repo URL prose + install command in "Cómo se trabaja en local").
- `README.md:7,13-17,23` — literal `adminoryslabs/Skills` in prose + 6 install command lines.
- `contracts/mcp-catalog-section.md` — referenced by the input spec as source of truth for the `type: mcp` frontmatter contract, but **does not exist** anywhere under this repo or under `D:/DMC_Courses/Armory/specs/mcp-catalog-section/` (only sibling spec files exist there: `registry-web.md`, `launcher-desktop.md`, `registry-api.md`, `adminoryslabs-skills.md`). This is a genuine missing dependency, not something explore should invent.
- GitHub repo rename (`adminoryslabs/Skills` → `adminoryslabs/Armory`) — out-of-band GitHub Settings action; this SDD change in this repo cannot execute it, only depends on it and updates literal references after it happens.

## Approaches

1. **Sequence: contract-first** — write `contracts/mcp-catalog-section.md` (defining `type: mcp` frontmatter: `name`, `description`, `type: mcp`, `tags`, `repo_url`, `docs_url`) as part of `sdd-propose`/`sdd-spec` for this change, before creating `mcps/*.md` files that reference it.
   - Pros: closes the missing-dependency gap explicitly; both files (`mcps/obsidian.md`, `mcps/github.md`) can cite a real, versioned contract; downstream `registry-api` proposal has something concrete to consume.
   - Cons: contract lives in the sibling `specs/mcp-catalog-section/` folder (per input spec's own reference path), which is outside this repo — coordination needed on where it's actually persisted (in `specs/` alongside sibling repo specs, vs. inside this repo's `openspec/changes/`).
   - Effort: Low.

2. **Parallel: infer contract inline, no separate contract file** — define the `type: mcp` frontmatter fields directly in this repo's spec/design without a standalone `contracts/mcp-catalog-section.md`.
   - Pros: faster, no cross-repo coordination.
   - Cons: contradicts the input spec's explicit reference to `contracts/mcp-catalog-section.md` as the thing other repos (`registry-api`) will consume — if `registry-api`'s own SDD cycle expects that file to exist, skipping it breaks the multirepo contract flow described in the spec's own "Contrato relacionado" section.
   - Effort: Low, but creates cross-repo drift risk.

## Recommendation

Approach 1 (contract-first). The input spec explicitly frames this repo as the one that "first produces both" the content convention and the contract that `registry-api` depends on — skipping the contract file to save time would just move the gap downstream into `registry-api`'s own SDD cycle, where it's harder to reconcile. `sdd-propose` should treat writing/locating `contracts/mcp-catalog-section.md` as an explicit task, and confirm with the user where it should physically live (this repo vs. the shared `specs/` folder) since neither location currently exists.

## Risks

- **Obsidian MCP has no single canonical repo.** Web search turned up multiple unrelated implementations (`cyanheads/obsidian-mcp-server`, `StevenStavrakis/obsidian-mcp`, `Vasallo94/obsidian-mcp-server`, `ZethicTech/obsidian-mcp`, `eddmann/obsidian-mcp`, etc.) with different tool surfaces. The user's own global CLAUDE.md references specific tool names (`mcp__obsidian__vault_read`, `vault_write`, `vault_patch`, `vault_append`, `vault_list`, `search_query`, `search_simple`) — that naming should be matched against candidate repos to identify which fork is actually in use before writing `mcps/obsidian.md`'s `repo_url`/`docs_url`. Guessing here would violate the spec's own "no inventar direcciones" requirement; per the spec's own instruction, mark it as pending confirmation if not resolved with certainty.
- **GitHub MCP is unambiguous**: `github.com/github/github-mcp-server` is confirmed as GitHub's own official server (verified via web search), so `mcps/github.md` has a low-risk path.
- **Contract file location is undecided** — `contracts/mcp-catalog-section.md` doesn't exist in this repo or in the sibling `specs/` folder; must be resolved before `sdd-tasks` can scope work precisely.
- **Rename sequencing risk** (already flagged by orchestrator, confirmed real): 6+ literal `adminoryslabs/Skills` occurrences across `CLAUDE.md` and `README.md` will break after the GitHub rename if not updated in the same change; `registry-api`'s sync will also fail post-rename until it's updated separately (out of scope for this repo but must be flagged downstream).

## Ready for Proposal

Yes — investigation surfaced concrete, evidence-backed gaps (missing contract file, non-uniform `type` field on skills, exact literal-URL locations, ambiguous Obsidian MCP repo identity) rather than assumptions. `sdd-propose` should explicitly scope: (a) `contracts/mcp-catalog-section.md` location decision, (b) `mcps/obsidian.md` blocked pending exact repo identification from the user, (c) `mcps/github.md` unblocked (repo_url confirmed), (d) README/CLAUDE.md literal-URL updates as a distinct task gated on the actual GitHub rename having happened.
