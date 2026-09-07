# spec.md template

Requirements-and-design spec for an approved `intent.md`, per Anthropic's AI-Native SDLC
playbook (https://claude.com/blog/the-ai-native-sdlc-playbook#sd-s2) and its companion Academy
lesson (https://academy.claude.com/courses/ai-native-sdlc-playbook/requirements-and-design).
Unlike `intent.md`, the source leaves spec.md's exact shape open — this is forge's chosen
shape, not a verbatim structure to defer to.

```
# Spec: <short name of the change>
Intent: <path to the source intent.md, e.g. intent/faster-checkout.md>
Author: claude. Status: draft.
Skills applied: <names of skills, AGENTS.md/CLAUDE.md sections, and/or knowledge-base pages
that actually shaped this spec, or "none found in this project" — never leave this line out>

## Summary
[What intent.md asked for and what got decided here, in a sentence or two — the bridge from
problem to requirements, not a restatement of either. Account for every one of intent.md's
Open questions: say how each was resolved, or carry it forward here unresolved — none may
silently disappear.]

## Requirements
REQ-1: [One concrete, testable requirement.]
Acceptance: [How to tell it's satisfied.]

REQ-2: [...]
Acceptance: [...]

## Design
[The technical approach: what changes, where, and why this approach over alternatives worth
naming. Concrete enough for an engineer to start from.]

## Areas of concern
[Policy conflicts, or brand/security/compliance/UX constraints this spec couldn't fully
satisfy — anything a policy owner needs to resolve before engineering starts. State plainly
that none were found if that's genuinely the case; don't leave the section blank by default.]
```

## Rules

- **Read the source intent, don't re-derive it.** Requirements come from the approved
  intent.md's Problem, Proposed outcome, and Affected users/systems — don't reopen questions
  intent.md already settled. A gap you find here is a gap in Intent, not something to
  freelance around in Spec.

- **Ask, don't guess.** When something needed for a confident Requirement or Design decision
  is genuinely unclear from intent.md alone, ask the operator directly — the same discipline
  intent.md itself is written with. Areas of concern is for conflicts a policy owner needs to
  weigh in on, not a substitute for a question you could just ask right now.

- **Every Open question gets accounted for.** Each one from the source intent.md is either
  resolved by a Requirement or Design decision, or explicitly carried forward in Summary — the
  playbook's own review question is "answered or carried forward?", never "dropped."

- **Gate on approval.** If the source intent.md's `Status` isn't `approved`, say so plainly
  and confirm with the user before proceeding. Spec is not the place to approve Intent
  retroactively.

- **This project's brand/security/compliance/UX policy is a prerequisite, not an
  afterthought.** The playbook lists it as required infrastructure for this stage — as skills,
  or as sections of `AGENTS.md`/`CLAUDE.md`. Check for both and apply whichever exist before
  drafting Requirements or Design — their constraints shape what gets written here, they are
  not a review pass at the end. If neither exists in this project, stop and confirm with the
  user before proceeding rather than drafting anyway; record whichever were actually applied
  in `Skills applied:`.

- **Consult this project's `knowledge-base/`, if it has one.** Domain concepts, prior
  architecture/ops decisions, and market context there should shape Requirements and Design —
  prefer the project's own query skill (e.g. `wiki-query`) over reading its files raw. Cite
  what you find inline the way its own convention does, and note which pages you drew on in
  `Skills applied:` too. Unlike policy skills, its absence isn't a gap to stop over — it's
  enrichment, not infrastructure the playbook requires.

- **Requirements are numbered and testable.** REQ-1, REQ-2, ... each one concrete enough that
  a later Build/Test stage can verify it, with an explicit Acceptance clause.

- **Design describes the approach, not just the outcome.** This is where implementation
  choices belong — the mirror image of intent.md's problem-first rule, not a continuation of
  it.

- **Areas of concern come first, not last.** The playbook treats a flagged policy conflict as
  the point a human must escalate before engineering starts — surface it explicitly rather
  than burying or smoothing it into Design.

- **Status follows the repo, same as Intent.** Every Spec starts `draft`. Move it to
  `approved` only once the product owner's actual approval event has happened — a merged
  commit or a closed review — and commit `spec.md` alongside its `intent.md`, not separately.
