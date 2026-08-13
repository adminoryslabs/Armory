---
name: create-harness
description: Guía al usuario, paso a paso y con preguntas, para diseñar y empaquetar su propio harness (skill instalable) para una tarea recurrente de su proyecto — revisión de deuda técnica, seguridad, despliegue, bug fixing, lo que necesite. Use when the user wants to build their own lightweight harness/skill for a recurring project task, or asks how to package a repeatable agent workflow as an installable skill.
---

# Harness Builder (Interactive)

## Goal

Guide the user through designing and packaging a **small, single-purpose harness** for a
recurring task on their own project — a debt review, a security check, a deploy checklist, a
bug-fixing routine, or anything else that follows a repeatable process. The output is an
installable `SKILL.md`, using the same convention as every other skill in this catalog, so a
teammate joining the project later can install it and get the same repeatable process.

**This skill is conversational, not one-shot.** Never assemble the final `SKILL.md` from a single
guess at what the user wants. Walk through each decision below one at a time, and write nothing
until the user has actually answered it.

**Scope discipline is the point of this skill.** A harness that tries to cover "everything" isn't
a harness, it's a wish list — it will drift, contradict itself, and be too vague to actually guide
an agent. If the user proposes something broad (security AND debt AND deploy, all in one), stop and
ask them to pick ONE concern for this pass. They can run this skill again later for the next one.

## Input

- A description, in the user's own words, of the recurring task this harness should handle —
  **required**. If it's vague ("something for code quality"), ask what specifically keeps
  happening that they want handled the same way every time.
- The project's existing `CLAUDE.md` / rules, if any — **optional but read them if present**. They
  tell you what context already exists vs. what this new harness will need to state explicitly.

## Workflow

1. **Purpose.** Ask what single, narrow task this harness should handle. If the answer bundles
   multiple unrelated concerns, push back per the Scope discipline note above and ask the user to
   choose one for this pass.
2. **Context required.** Ask what the agent needs to read every time this harness runs to do the
   task well (specific files, folders, existing docs, external references). If the project's
   `CLAUDE.md` is missing something this harness will depend on, say so explicitly — do not assume
   the agent will "figure it out."
3. **Workflow steps.** Ask the user to walk you through the actual process, step by step, in the
   order it should happen. This is domain knowledge only the user has — do not invent a generic
   process and present it as theirs. Number the steps as they describe them.
4. **Output format.** Ask what the harness should produce each time it runs — a written report, a
   pass/fail checklist, inline findings, a file written to a specific path. Get a concrete shape,
   not "a summary."
5. **Guardrails.** Ask explicitly: "Is there anything this harness should never do without asking
   you first?" If the user names something, encode it as an explicit stop-and-ask condition inside
   the Workflow section of the resulting skill — not as a passing mention in prose. If the user says
   no, skip this without inventing a guardrail they didn't ask for.
6. **Assemble and write** `skills/{harness-slug}/SKILL.md` in the user's own project, following the
   same frontmatter convention as this catalog:

   ```yaml
   ---
   name: {harness-slug}
   description: {short description in the project's language} Use when {trigger condition in English}.
   ---
   ```

   The body (Goal, Input, Workflow, Quality Gate) is written in English, matching the convention of
   every other skill in this catalog, so it stays consistent if it's ever published or shared.

## Quality Gate

Before writing the final file, silently check:

- The harness has exactly one clear purpose — not several concerns bundled together.
- Every workflow step came from the user's actual answer, not invented to fill a gap.
- The output format is concrete enough that two different runs would produce comparably shaped
  results.
- Any guardrail the user named is an explicit stop-and-ask condition in the Workflow, not just
  mentioned once and forgotten.
- The frontmatter matches the same convention as the rest of the catalog — so it could be installed
  the same way as any other skill if the user chooses to share it later.

## After this skill

The user now has both a working harness and the process to build another one. Remind them at the
end: the same set of questions applies to any other recurring task they identify later — a
different harness for bug-fixing, a lighter SDD variant, a deploy checklist. Running this skill
again, once per concern, is the expected way to grow their own set of harnesses over time.
