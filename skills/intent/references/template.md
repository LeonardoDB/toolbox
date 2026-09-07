# intent.md template

Canonical shape for `intent.md`, straight from Anthropic's AI-Native SDLC playbook
(https://claude.com/blog/the-ai-native-sdlc-playbook) and its companion Academy lesson
(https://academy.claude.com/courses/ai-native-sdlc-playbook/capture-intent) — not a
suggestion to riff on.

```
# Intent: <short name of the change>
Author: <person, or "claude (maintain scan)" for evidence-based intents>. Status: draft.
Source: <original issue tracker URL — omit this line entirely if there isn't one>

## Problem
[What can't be done today, or what's broken. One paragraph — not a requirements list.]

## Proposed outcome
[The better state, described as an outcome, not a design. Leave under Open questions if
genuinely undecided rather than inventing a direction.]

## Affected users and systems
[Who is affected; which services/modules/repos are in the blast radius.]

## Constraints
[Regulatory, technical, or org limits this has to respect. Empty section is fine — don't
pad it.]

## Open questions
[Uncertainties for Spec to resolve. This is where "I don't know yet" belongs — never leave
an unresolved unknown silently folded into Problem or Proposed outcome.]
```

## Rules

- **Problem-first, not solution-first.** Neither Problem nor Proposed outcome is the place
  for numbered requirements, an implementation approach, or a design — Proposed outcome
  describes what becomes better, not how:

  - Prefer: `Users can retry failed imports without restarting the whole import.`
  - Avoid: `Add a retry endpoint backed by a Redis queue.`

  If a sentence reads like a requirement, implementation choice, or design decision, move it
  to Open questions until Spec exists.

- **The originator's own words, not formal requirement-speak.** Intent should read the way
  the person actually described the problem — don't translate it into analyst or spec
  language before Spec exists.

- **Brainstorm before drafting, don't draft while brainstorming.** Ask the questions an
  analyst would ask first — scope, affected users/systems, constraints, what success looks
  like — until the idea is concrete, then write the whole thing in one pass, not section by
  section as answers trickle in.

- **Short.** A few sentences per section. A section needing a wall of text, or an Intent
  describing more than one independent problem, is probably two intents.

- **Cite your evidence.** When Problem or Affected users/systems comes from logs, telemetry,
  or an issue tracker rather than directly from a person, cite it inline so a reviewer can
  verify the claim instead of trusting it blind — a log excerpt, an `error rate spike in
  file:line`, or a close paraphrase of what the issue actually said, never a reconstruction
  from memory.

- **Don't invent unknowns away.** Genuinely unclear scope, behavior, success criteria, or
  direction goes under Open questions — never silently folded into Problem or Proposed
  outcome as if it were settled.

- **Constraints are real constraints only.** Regulatory, technical, compatibility,
  organizational, operational, or platform limits belong here; an empty section is valid,
  don't invent one to fill it.

- **`Status` follows the repo, not a separate field.** Every Intent starts `draft`. Move it
  to `approved` only once the repo's own approval event has actually happened — a merged
  commit or a closed review is the approval record; don't invent a workflow the repo doesn't
  have.

- **Intent is not Spec.** No Requirements, Acceptance criteria, Implementation, Architecture,
  API design, Tasks, Rollout plan, or Test plan sections — those belong to later stages.
