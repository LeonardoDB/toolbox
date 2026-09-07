---
name: spec
description: Turn an approved intent.md into a requirements-and-design spec.md — numbered requirements, a technical approach, and explicitly flagged brand/security/compliance/UX concerns. Use once an intent has been approved and it's time to plan how to build it. Second stage of the forge loop.
---

# Spec

You turn *what* and *why* (an approved `intent.md`) into *how* — numbered requirements and a
design approach a team can build from, applying this project's own brand, security,
compliance, and UX policy as constraints while you write, not as a review afterward.

The exact template is in `references/template.md` — read it before writing, and keep its
exact section set and order: no improvising, renaming, or reordering sections. Its Rules
section carries the full reasoning behind each step below — these are just the sequence.

**Prerequisites:** an `intent.md`, and this project's brand, security, compliance, and UX
policies written down somewhere Claude actually reads — as skills, or as sections of this
project's `AGENTS.md`/`CLAUDE.md`. The playbook treats this as required infrastructure for
this stage, not optional — if this project has neither, that's a real gap, not a detail to
note in passing (see step 2).

**Who this is for:** the playbook casts this as the product owner's session, not the
engineer's — "a product owner with Claude access, no engineering skill required." Engineering
enters at Build, once `spec.md` is accepted.

**Invocation:** `/forge:spec [path-to-intent.md]`. Given a path, use that intent.md. Left
blank, look in this project's `intent/` folder: exactly one file → use it; more than one →
ask which; none → tell the user to run `/forge:intent` first and stop.

## What to do

1. Read the source `intent.md` in full. Gate on its `Status: approved` — stop and confirm
   with the user if it isn't.
2. Check this project for brand/security/compliance/UX policy (skills, or its
   `AGENTS.md`/`CLAUDE.md`) and apply whatever exists before drafting. Stop and confirm with
   the user if neither exists.
3. Consult this project's knowledge base, if it has one — enrichment, not a gate. Default
   location: a `knowledge-base/` folder at the project root; this project's `.claude/forge.md`
   overrides the path (and names a query skill to prefer) if its **Knowledge base** section
   says so.
4. Ask the operator directly for anything still genuinely unclear; don't guess.
5. Account for every one of the source intent.md's Open questions — each resolved or
   explicitly carried forward.
6. Fill the template from `references/template.md` in full. Record every source that actually
   shaped the spec — skills, `AGENTS.md`/`CLAUDE.md`, knowledge-base pages — in `Skills
   applied:`.
7. Derive the same kebab-case slug the source intent.md uses and write the file to
   `intent/<slug>.spec.md`, alongside it — never forge's own plugin directory.
8. Stop. Do not proceed to implementation, and do not write a `plan.md` — that's Plan's own
   artifact (Claude Code's Plan Mode, triggered by an accepted spec, not this skill's job:
   `/forge:plan`, or `/forge:run` to drive the whole chain).
