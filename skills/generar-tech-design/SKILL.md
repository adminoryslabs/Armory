---
name: generar-tech-design
description: Genera el Technical Design Document + ADRs (formato MADR) + criterios de aceptación de un proyecto, entrevistando al usuario decisión por decisión a partir de su PRD y Design.md. Use when the user asks to generate, draft, or create a technical design document, system architecture, or architecture decision records (ADRs) for a project.
---

# Technical Design Document Generator (Interactive)

## Goal

Produce a production-grade Technical Design Document (TDD) and its supporting Architecture
Decision Records (ADRs, MADR format) — an artifact solid enough to take into a real company, not
a course exercise. This is not a light draft generator.

**This skill is conversational, not one-shot.** Never generate the full TDD in a single pass. Walk
the user through each architecture decision one at a time, present real alternatives with
trade-offs, ask which one they choose (or if they have a different idea), and only then write the
ADR reflecting their actual decision. An ADR the agent decided alone, without the user's input, is
a failure of this skill.

## Required Input

- `PRD.md` (required) — problem, users, scope, product-level acceptance criteria.
- `Design.md` (required) — UI/UX and, critically, the concrete data the interface implies (a
  discount field, a status badge, a filter) — a primary input for data model decisions.

If either is missing, stop and ask for it. Do not fabricate PRD/Design content — only architecture
decisions belong to this skill.

## Workflow

1. Read `PRD.md` and `Design.md` fully.
2. Determine the decision areas that apply to this project from `assets/decision-areas.md`
   (components/repos, data model, API contracts, tech stack per component, state management,
   resilience/error handling, plus anything the PRD forces that isn't on that list).
3. For each decision area, **one at a time**:
   a. State the context: why this decision is needed, quoting what the PRD/Design.md implies.
   b. Present 2-3 real alternatives with concrete trade-offs (not a false choice — each option must
      be genuinely viable for this project).
   c. Ask the user which one they choose, or whether they have a different option in mind. Wait
      for the answer before continuing.
   d. Write the ADR immediately using `assets/adr-template.md`: the chosen option as Decision, the
      others as Alternatives considered (with the reason they were not chosen), and Consequences
      that include at least one real trade-off of the choice made, not only benefits.
4. After all decisions are recorded, derive acceptance criteria per key flow in the PRD, at a more
   granular level than the PRD's own criteria. Confirm with the user before finalizing if any
   criterion required a judgment call.
5. Assemble `TECH-DESIGN.md` from `assets/tech-design-template.md`, referencing every ADR by
   number.
6. Write to disk:
   - `TECH-DESIGN.md` at the project root.
   - `adrs/0001-{slug}.md`, `adrs/0002-{slug}.md`, ... one file per decision, numbered sequentially
     in the order they were made.

## Quality Gate

Before closing, silently check:

- Every ADR has Context, Decision, at least one rejected Alternative, and Consequences with at
  least one real cost or trade-off.
- No architecture decision contradicts the PRD's "No alcance" section — if one would, surface it to
  the user instead of silently resolving it.
- Every UI element in `Design.md` that implies data has a corresponding data model decision.
- Acceptance criteria are verifiable, not vague adjectives.
- Every decision in `TECH-DESIGN.md` was actually answered by the user — not filled in solo by the
  agent because a question was skipped.

## References

- `assets/decision-areas.md` — which decisions to walk through, and when to add ones not listed.
- `assets/adr-template.md` — MADR-lite format for each ADR.
- `assets/tech-design-template.md` — the TDD document that assembles and references the ADRs.
