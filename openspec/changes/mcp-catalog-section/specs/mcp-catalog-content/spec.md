# MCP Catalog Content Specification

## Purpose

Define the `mcps/<name>.md` content convention — a third entry type alongside
`skills/` and `prompts/` — so MCP servers can be cataloged with install
prompts that are safe to hand to an agent (no invented setup steps, no fake
credentials).

## Requirements

### Requirement: MCP Entry File Convention

Each MCP catalog entry MUST live at `mcps/<name>.md` (single file, mirroring
the `prompts/<name>.md` convention — not the `skills/<name>/SKILL.md`
directory convention). The frontmatter MUST conform exactly to the external
contract at `D:/DMC_Courses/Armory/contracts/mcp-catalog-section.md`:
`name`, `description`, `type: mcp`, `tags`, `repo_url`, `docs_url`. The file
body MUST be a hand-written install prompt, not dynamically generated.

#### Scenario: Valid MCP entry file

- GIVEN a new MCP entry is added to the catalog
- WHEN the file is created at `mcps/<name>.md`
- THEN its frontmatter contains `name`, `description`, `type: mcp`, `tags`,
  `repo_url`, and `docs_url` (when available) matching the external contract shape
- AND the body is a hand-written install prompt, not a generated stub

#### Scenario: Entry placed under skills/ directory convention by mistake

- GIVEN a contributor attempts to add an MCP entry as `skills/<name>/SKILL.md`
- WHEN the entry is reviewed against this convention
- THEN it MUST be rejected/relocated to `mcps/<name>.md`

### Requirement: GitHub MCP Catalog Entry

The catalog MUST include `mcps/github.md` with `repo_url:
https://github.com/github/github-mcp-server`, valid `type: mcp` frontmatter,
and no placeholder or fabricated URL values.

#### Scenario: GitHub MCP entry exists with correct repo_url

- GIVEN the catalog is built
- WHEN `mcps/github.md` is read
- THEN `repo_url` equals `https://github.com/github/github-mcp-server`
- AND `type` equals `mcp`

### Requirement: Obsidian MCP Catalog Entry

The catalog MUST include `mcps/obsidian.md` with `repo_url:
https://github.com/coddingtonbear/obsidian-local-rest-api` and `docs_url:
https://coddingtonbear.github.io/obsidian-local-rest-api/`.

#### Scenario: Obsidian MCP entry exists with correct repo_url and docs_url

- GIVEN the catalog is built
- WHEN `mcps/obsidian.md` is read
- THEN `repo_url` equals `https://github.com/coddingtonbear/obsidian-local-rest-api`
- AND `docs_url` equals `https://coddingtonbear.github.io/obsidian-local-rest-api/`

### Requirement: Install Prompt Safety Behavior

Every MCP install prompt body MUST instruct the consuming agent to check the
official README before installing (never assume memorized instructions are
current), MUST instruct the agent to stop and explicitly ask the user for any
mandatory external setup step (OAuth app registration, token creation,
enabling a plugin/service) rather than inventing or silently skipping it, and
MUST use explicit credential placeholders (e.g. `<TU_TOKEN_AQUI>`) — never
fake-realistic values that could be mistaken for real credentials.

#### Scenario: Agent follows install prompt for an entry requiring a token

- GIVEN an agent is installing an MCP using its catalog install prompt
- WHEN the prompt reaches a step requiring an external token or app registration
- THEN the agent stops and asks the user for that credential
- AND the prompt text shows an explicit placeholder, not a realistic-looking fake value

#### Scenario: Prompt references outdated setup steps

- GIVEN the official README of the MCP has changed since the prompt was written
- WHEN the agent follows the install prompt
- THEN the prompt instructs the agent to verify current steps against the official README before proceeding

### Requirement: Obsidian Install Prompt Transport and Credential Handling

The Obsidian install prompt body MUST clarify that installation is an
Obsidian plugin action (via Community Plugins or BRAT), not an npx/npm
process, and MUST instruct the agent to stop and ask the user for: plugin
enabled-confirmation, the bearer token from plugin settings, and the HTTP vs
HTTPS transport choice — never inventing any of the three.

#### Scenario: Agent installs the Obsidian MCP

- GIVEN an agent follows `mcps/obsidian.md`
- WHEN it reaches the setup step
- THEN it asks the user to confirm the plugin is enabled, provide the bearer token, and choose HTTP or HTTPS
- AND it does not attempt an npx/npm install for this entry

### Requirement: Gated Rename Reference Update

Updates to literal `adminoryslabs/Skills` references in `README.md` and
`CLAUDE.md` MUST occur only after the user explicitly confirms the GitHub
repo rename to `adminoryslabs/Armory` has already happened.

#### Scenario: Rename confirmed by user

- GIVEN the user explicitly confirms the GitHub rename has happened
- WHEN this change is applied
- THEN `README.md` and `CLAUDE.md` literal `adminoryslabs/Skills` references are updated to `adminoryslabs/Armory`

#### Scenario: Rename not confirmed

- GIVEN the user has not confirmed the GitHub rename
- WHEN this change is applied
- THEN `README.md` and `CLAUDE.md` are left unchanged
- AND a note is left indicating the update is pending user confirmation
