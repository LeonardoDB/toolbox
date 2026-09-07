---
name: plan
description: Turn an approved intent.md + spec.md pair into an implementation plan.md via Claude Code's own Plan Mode — files that change, order of work, risks, and proof. Use once a spec has been approved and it's time to start Build. Third stage of the forge loop.
---

# Plan

You turn intent + spec into the concrete plan an engineer would need to implement from,
without re-reading either source document — produced in Claude Code's own Plan Mode, not as
a freestanding write-up.

The exact template is in `references/template.md` — read it before writing, and keep its
exact section set and order: no improvising, renaming, or reordering sections.

**Prerequisites:** an `intent.md` and its `spec.md`, both `Status: approved`. `CLAUDE.md`
helps but isn't required.

**Who this is for:** the playbook casts this as the engineer's session — the first stage in
forge's loop where engineering, not product ownership, is in the seat.

**Invocation:** `/forge:plan [path-to-intent.md]`. Given a path, use that intent.md and its
sibling `<slug>.spec.md`. Left blank, look in this project's `intent/` folder the same way
Spec does.

## What to do

1. Work in Plan Mode for this whole skill — enter it if the session isn't already in it.
   This isn't a formatting choice: the plan's real control is that Claude cannot edit files
   until the plan is accepted, and that only holds inside Plan Mode.
2. Read the source `intent.md` and `spec.md` in full. Gate on both being `Status: approved`
   — stop and confirm with the user if either isn't.
3. Explore the actual codebase (read-only) to build an impact map before naming anything:
   which module/service is the primary home (and why — cite the actual convention or its
   `CLAUDE.md` rule if one exists), whether this is a write path or a read path, whether a
   schema/migration is touched and what needs to refresh afterward, and any cross-cutting
   risk (a forked write path, event coupling). This is what **Files that change** and
   **Risks** below get built from — not a generic approach to the problem, and not a new
   section of its own: `plan.md`'s shape stays exactly what `references/template.md` says.
4. Interrogate the plan before presenting it: what could break, which step is riskiest, and
   what alternative you rejected and why. Fold the answers into the plan itself, not just
   into the conversation.
5. Fill the template from `references/template.md` in full, to the completeness bar it
   states: someone unfamiliar with this conversation could implement from the plan alone.
6. Derive the same kebab-case slug the source intent.md uses and write the file to
   `intent/<slug>.plan.md`, alongside its intent and spec — never forge's own plugin
   directory.
7. Stop. This skill produces `plan.md` only — accepting it and implementing from it is
   normal Claude Code work after this session, not something this skill does. If
   implementation ever departs from the plan, `plan.md` should be updated in the same commit
   as the departure; that update isn't this skill's job either, but say so if asked.
