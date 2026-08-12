# Tasks: MCP Catalog Section

Spec: `openspec/changes/mcp-catalog-section/specs/mcp-catalog-content/spec.md`
Design: `openspec/changes/mcp-catalog-section/design.md`

## 0. Blocking pre-work

- [x] **0.1** Ask the user to confirm whether the GitHub repo rename
  (`adminoryslabs/Skills` -> `adminoryslabs/Armory`) has already happened,
  before starting the `README.md`/`CLAUDE.md` edit tasks. This is a blocking
  question for `sdd-apply` — do not infer or guess the answer, and do not
  start section 4 or 5 until it is answered.
  _Satisfies: Requirement "Gated Rename Reference Update" (spec.md:99-116)._

## 1. Folder setup

- [x] **1.1** Create the `mcps/` folder at repo root (single-file convention,
  mirrors `prompts/<name>.md` — not the `skills/<name>/SKILL.md` directory
  convention).
  _Satisfies: Requirement "MCP Entry File Convention" (spec.md:12-33)._

## 2. `mcps/github.md`

- [x] **2.1** Verify whether `github-mcp-server` has a real, distinct
  `docs_url` (beyond the repo README itself). If a genuine, separate docs
  site exists, use it; if none exists, omit the `docs_url` field entirely
  per the contract (it is optional) — do not fabricate one.
  _Design note: Architecture Decisions row "`docs_url` on `github.md`"
  (design.md:17); Open Questions (design.md:115-116)._
- [x] **2.2** Write `mcps/github.md` frontmatter: `name: github`,
  `description` (short, Spanish or English per existing convention),
  `type: mcp`, `tags: [github, git, mcp]`, `repo_url:
  https://github.com/github/github-mcp-server`, and `docs_url` only if
  task 2.1 found a real value.
  _Satisfies: Requirement "GitHub MCP Catalog Entry" (spec.md:35-46);
  Requirement "MCP Entry File Convention" (spec.md:12-33)._
- [x] **2.3** Write `mcps/github.md` body as a hand-written install prompt
  (Spanish prose, no markdown headers, same register as
  `prompts/ejecutar-multirepo.md`), following the 5-paragraph skeleton:
  Contexto, Verificación previa (check `repo_url` for current install
  method, never assume the prompt's steps are still current), Prerrequisitos
  (stop and ask the user for the access token — scopes per the official
  repo — using an explicit placeholder like `<TU_TOKEN_AQUI>`, never a
  realistic-looking fake value), Instalación/configuración, Verificación
  final.
  _Satisfies: Requirement "MCP Entry File Convention" (spec.md:12-33);
  Requirement "Install Prompt Safety Behavior" (spec.md:61-82)._

## 3. `mcps/obsidian.md`

- [x] **3.1** Write `mcps/obsidian.md` frontmatter: `name: obsidian`,
  `description` (short), `type: mcp`, `tags: [obsidian, notes, mcp,
  plugin]`, `repo_url:
  https://github.com/coddingtonbear/obsidian-local-rest-api`, `docs_url:
  https://coddingtonbear.github.io/obsidian-local-rest-api/`.
  _Satisfies: Requirement "Obsidian MCP Catalog Entry" (spec.md:48-59)._
- [x] **3.2** Write `mcps/obsidian.md` body as a hand-written install prompt
  (same 5-paragraph skeleton and register as 2.3), clarifying that
  installation is an Obsidian plugin action (Community Plugins or BRAT),
  not an npx/npm process, and instructing the agent to stop and ask the
  user for all three of: (a) plugin `Local REST API` installed/enabled
  confirmation, (b) the bearer token from plugin settings, (c) HTTP
  (port 27123) vs HTTPS (port 27124, default, self-signed cert) transport
  choice — never inventing any of the three.
  _Satisfies: Requirement "Obsidian Install Prompt Transport and Credential
  Handling" (spec.md:84-97); Requirement "Install Prompt Safety Behavior"
  (spec.md:61-82)._

## 4. Rename-confirmation gate (conditional on task 0.1)

- [x] **4.1** Apply the rename-confirmation gate rule: only an explicit
  "yes" from the user (from task 0.1) authorizes editing `README.md` and
  `CLAUDE.md`. Any other answer — explicit "no", "not yet", silence,
  ambiguity, or anything short of an unambiguous "yes" — MUST be treated as
  "no": skip both files entirely and leave a note in the apply summary
  ("README.md/CLAUDE.md rename edits pending — apply again after confirming
  the GitHub rename"). Do not touch either file in that case.
  _Satisfies: Requirement "Gated Rename Reference Update" (spec.md:99-116),
  scenario "Rename not confirmed" (spec.md:111-116)._

## 5. `README.md` edit (only if task 0.1 confirmed "yes")

- [x] **5.1** Replace literal `adminoryslabs/Skills` with
  `adminoryslabs/Armory` at the exact known locations in `README.md`
  (line 7, lines 13-17, line 23). Do not touch unrelated stale content in
  the same file (e.g. the missing 6th skill in the list) — out of scope.
  _Satisfies: Requirement "Gated Rename Reference Update" (spec.md:99-116),
  scenario "Rename confirmed by user" (spec.md:105-109)._
- [x] **5.2** Grep `README.md` for `adminoryslabs/Skills` after editing —
  must return zero matches.
  _Design: Testing/Validation Strategy, "Post-edit literal check"
  (design.md:106)._

## 6. `CLAUDE.md` edit (only if task 0.1 confirmed "yes")

- [x] **6.1** Replace literal `adminoryslabs/Skills` with
  `adminoryslabs/Armory` at the exact known locations in `CLAUDE.md`
  (line 3 as `github.com/adminoryslabs/Skills`, plus lines 13, 33, 36). Do
  not touch unrelated content in the same file — out of scope.
  _Satisfies: Requirement "Gated Rename Reference Update" (spec.md:99-116),
  scenario "Rename confirmed by user" (spec.md:105-109)._
- [x] **6.2** Grep `CLAUDE.md` for `adminoryslabs/Skills` after editing —
  must return zero matches.
  _Design: Testing/Validation Strategy, "Post-edit literal check"
  (design.md:106)._

## 7. Final validation

- [x] **7.1** Read back `mcps/github.md` and `mcps/obsidian.md` frontmatter
  against the external contract at
  `D:/DMC_Courses/Armory/contracts/mcp-catalog-section.md` — confirm exact
  field shape (`name`, `description`, `type: mcp`, `tags`, `repo_url`,
  `docs_url` when present).
  _Satisfies: Requirement "MCP Entry File Convention" (spec.md:12-33)._
- [x] **7.2** Manually review both install prompt bodies against the
  Install Prompt Safety Behavior requirement: official-README verification
  step present, explicit stop-and-ask for mandatory external setup, and
  only explicit placeholders (never realistic-looking fake credentials).
  _Satisfies: Requirement "Install Prompt Safety Behavior" (spec.md:61-82)._

## Ordering notes

- Tasks in section 0 must run first — task 0.1's answer gates sections 4-6
  entirely.
- Sections 1-3 (folder + both `mcps/*.md` entries) are independent of the
  rename gate and can proceed in parallel with, or before, task 0.1.
- Within section 2, task 2.1 (docs_url research) must complete before 2.2
  (frontmatter write).
- Section 4 (gate application) must run before sections 5 and 6.
- Section 5 and section 6 are independent of each other and can run in
  parallel once section 4 confirms "yes".
- Section 7 (final validation) runs last, after all preceding sections.

## Parallelizable work

- Task 2.1-2.3 (`github.md`) and task 3.1-3.2 (`obsidian.md`) can be done in
  parallel — no shared state.
- Section 5 (`README.md`) and section 6 (`CLAUDE.md`) can be done in
  parallel once gated open by section 4.
- Task 0.1 (user question) can be asked concurrently with sections 1-3
  since it only gates sections 4-6.

## Review Workload Forecast

- **Estimated changed lines:** ~90-130 lines total.
  - `mcps/github.md`: new file, ~25-35 lines (frontmatter + 5-paragraph body).
  - `mcps/obsidian.md`: new file, ~30-40 lines (frontmatter + 5-paragraph
    body, slightly longer due to the 3-item stop-and-ask gate).
  - `README.md`: ~7 line replacements, conditional on rename confirmation.
  - `CLAUDE.md`: ~4 line replacements, conditional on rename confirmation.
- **Chained PRs recommended:** No. All changes are additive Markdown content
  plus at most 11 gated literal-string replacements across two files — well
  under the 400-line threshold even in the worst case (rename confirmed).
- **400-line budget risk:** Low. Even with both conditional file edits
  applied, total changed lines stay well under 400.
- **Decision needed before apply:** Yes — but it is the rename-confirmation
  question (task 0.1), not a PR-splitting decision. This is a content-scope
  gate, not a delivery-strategy gate; `delivery_strategy: single-pr` is
  confirmed appropriate for this change and requires no `size:exception`.
