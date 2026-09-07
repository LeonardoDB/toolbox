---
name: intent
description: Capture what's broken or wanted and why, before any requirement is written. Produces intent.md (problem, proposed outcome, affected users/systems, constraints, open questions). Use when starting new work from a description, from an issue tracker URL, or when generating intent from log/incident evidence during an automated maintenance scan. First stage of the forge loop — the only stage forge implements today.
---

# Intent

You capture *why* a piece of work should happen, before anything is designed or built — the
first artifact in forge's loop, and currently the only stage forge implements.

The exact template is in `references/template.md` — read it before writing, and keep its
exact section set and order: no improvising, renaming, or reordering sections.

**Prerequisites:** none. Intent is deliberately the entry point — no template setup, no
existing artifact, nothing else needs to exist first.

**Who this is for:** the playbook casts this as the originator's session — typically product
management/product ownership, or whoever noticed the problem — not engineering. No engineering
skill is required to run this.

**Invocation:** `/forge:intent [description | issue-url]`. Anything after the command name is
either a starting problem statement (Mode A) or an issue tracker URL (Mode C) — treat
whichever it is as already-given and don't re-ask what it states. Left blank, start the
interview from scratch.

## Three modes

**Mode A — Interactive (default).** A person describes a problem or a feature idea, often in
one or two sentences. Ask only what you need to fill the template's sections — skip a
question whose answer is already implied by what was given. If something is genuinely
undecided, leave it under **Open questions** rather than inventing an answer to look
complete.

**Mode B — Evidence-based (maintenance scan).** No one is present; you were handed log
excerpts, error reports, an alert, or an incident ID instead of a conversation. Infer
**Problem** and **Affected users and systems** from that evidence and **cite it inline** (log
excerpt, `file:line`, error rate) so whoever reads this later can verify the claim instead of
trusting it blind. Set `Author: claude (maintain scan)`. Leave **Proposed outcome** and
**Constraints** under Open questions if the evidence doesn't actually support a fix direction —
a plausible guess dressed as a decision is worse than an honest unknown here. (Forge doesn't
yet run this mode on a schedule — see the plugin README's roadmap. Today this mode exists so
the skill behaves correctly whenever that scheduling is wired up, and so it can be exercised
by hand by pasting in log/incident text.)

**Mode C — Tracker-sourced (issue tracker URL).** Read
`${CLAUDE_PLUGIN_ROOT}/references/trackers.md` before doing anything else and follow it in
full for *how to fetch* the issue. Once you have it: treat its description as you would a
person's problem statement in Mode A (ask clarifying questions only for gaps it doesn't
answer), treat the comment thread as supporting evidence you cite the way Mode B cites logs,
and add a `Source:` line to the written intent.md pointing at the original issue — forge's
"linkage minimum" between repo and tracker (see the README's "Artifacts" section), not an
attempt to keep the two in sync afterward.

## What to do

1. Determine the mode (Mode A, B, or C above) from what you were given.
2. Fill the template from `references/template.md`, following its rules in full — problem-first
   above all, per that file. Keep each section to a few sentences — if one needs a wall of
   text, it's probably two intents.
3. Derive a kebab-case slug from the problem's short name and write the file to
   `intent/<slug>.md` at the root of the project you're working in (create the `intent/`
   folder if it doesn't exist yet). This is the target project's `intent/`, never forge's own
   plugin directory.
4. Stop. Intent only produces `intent.md` — tell the user plainly that turning it into a spec
   is a separate step (`/forge:spec`, once this intent is approved), not something to do here.
