# forge configuration — template

Copy this file into the target project as **`.claude/forge.md`** and fill in what applies.
Every skill in this plugin reads it first if present, and treats it as overriding the
defaults noted below. It's prose read by the model, not parsed config — write it the way
you'd brief a new teammate.

Every section is **optional**. Delete what you don't need; anything absent falls back to the
default noted per section. Keep entries short and imperative — each line here is an
instruction the skills will follow verbatim.

Sub-agents `loop` dispatches don't read this file themselves — `loop` pastes the relevant
section into each brief.

---

## Worker guardrails

<!-- Default when absent: no project-specific guardrails — the implementer sub-agent in
     skills/loop/SKILL.md carries only its own built-in discipline (branch check, no commit,
     evidence in the report). -->

Hard rules for anyone editing this repo, pasted **verbatim** into every implementer brief —
`loop` requires them word-for-word, because a summarized guardrail is a broken one. One
imperative line per rule, no rationale, no grouping:

- <e.g. "Never run a repo-wide formatter or a linter with --fix; the read-only gate is
  `<script>`">
- <e.g. "Build with `<dev build script>`; the release script belongs to CI, never to a
  worker">
- <e.g. "The git index is the user's review record: never run `git add`, `git reset`,
  `git stash`, or `git checkout -- <path>`">

## Branching

<!-- Default when absent: `<type>/<slug>` (type in feat | fix | chore), off the detected
     integration branch — see skills/loop/references/workspace.md. -->

- Pattern: <e.g. `<type>/<issue-id>-<slug>`, prefixes, forbidden characters>
- Examples: <two or three real-shaped but neutral examples>

## Knowledge base

<!-- Default when absent: a `knowledge-base/` folder at the target project's root — see
     spec's SKILL.md. -->

- Path: <where it actually lives, if not the default>
- Query skill: <the skill name to prefer, if this project ships one, e.g. `wiki-query`>

## Ledger

<!-- Default when absent: `intent/ledger.md` — see references/ledger.md. -->

- Path: <where it actually lives, if not the default>
- Promotion targets: <where a promoted entry can land beyond this project's CLAUDE.md or
  knowledge base — a specific wiki page, a style skill's SKILL.md, etc.>

## Security-sensitive areas

<!-- Default when absent: auth, secrets, payments. -->

- <domains in this codebase that must trigger `/security-review` rather than the
  implementer's normal feedback loop, e.g. billing, PII exports>

## Autonomy

<!-- Default when absent: `ask` — every stage stops for the user's explicit approval before
     advancing, exactly `loop`'s built-in behavior. -->

- Mode: <ask | executive>
- Executive contract (applies only when mode is `executive`): `loop` and the stage skills
  make and RECORD routine calls themselves instead of asking — an Intent question gets its
  best-supported answer folded straight into the relevant section, a Spec gap gets decided
  the same way in Summary/Requirements/Design — and each stage's report to the user says
  what was assumed and why, so the user can veto before the next stage starts instead of
  before this one. It still stops and asks: a one-way door (schema, public API, shared write
  path, data migration), a change to user-visible scope, anything named under
  Security-sensitive areas above, and a genuine 50/50 it cannot form a recommendation for.
  Plan always stops regardless of mode — Plan Mode's own acceptance is the actual control
  there, and no config setting skips a harness-level gate.
- Extra escalations: <project-specific decisions that must always go to the user, if any>
