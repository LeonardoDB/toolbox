# plan.md template

Verbatim structure from Anthropic's AI-Native SDLC playbook
(https://claude.com/blog/the-ai-native-sdlc-playbook#sd-s3) — like `intent.md`, and unlike
`spec.md`, the source gives this shape directly. This is the canonical shape, not a
suggestion to riff on.

```
# Plan: <short name of the change> (from intent.md <date>)

## Files that change
[Every file that changes, new or modified, with a one-clause reason each — not just a list of
paths.]

## Order of work
[Numbered steps, in the order they'll actually happen. Each step should be small enough to
verify on its own.]

## Risks
[What could break, which step is riskiest, and which alternative approach was rejected and
why. This section is the point of interrogating the plan before presenting it — an empty
Risks section usually means the plan wasn't interrogated, not that there are none.]

## Proof
[How completion will be verified: which tests, which screenshot against which approved mock,
which manual check. Concrete enough that "done" isn't a judgment call.]
```

## Rules

- **Self-sufficient or it's not done.** The playbook's own bar: "an engineer unfamiliar with
  the conversation could implement from the plan alone" — without opening intent.md or
  spec.md again. If a step only makes sense with context from the conversation that produced
  it, that context belongs in the plan, not left implicit.

- **Real files, not placeholders.** Read the actual codebase before naming what changes —
  this plan is about *this* repo's real files and structure, not a generic description of the
  approach that could apply to any codebase. Before listing files, know where the change
  actually lives: the primary module/service (and the convention that puts it there), whether
  it's a write path or a read path, whether a schema/migration is involved and what needs to
  refresh afterward, and any cross-cutting risk (a forked write path, event coupling). That's
  what **Files that change** and **Risks** get built from — it doesn't earn its own section;
  the template's shape stays exactly four headers.

- **Risks come from interrogating the plan, not from padding it.** Ask what could break,
  which step is riskiest, and what alternative got rejected — genuinely, before writing this
  section — rather than inventing generic risks to avoid an empty section.

- **Order of work is small, verifiable steps, not phases.** Each step should be checkable on
  its own before moving to the next, not a broad phase that hides several unrelated changes.

- **Plan Mode is the control, not the file.** The governance the playbook relies on here is
  that Claude cannot edit files until the plan is accepted inside Plan Mode — `plan.md` is
  the committed record of that acceptance, not a substitute for it. A `plan.md` written
  outside Plan Mode hasn't actually gone through the gate it's supposed to represent.

- **Keep it in sync with what actually happened.** If implementation ends up departing from
  the plan, `plan.md` gets updated in the same commit as the departure — a stale plan next to
  a diverging diff defeats the audit trail the playbook is built on.

- **This history is the metrics, not just a record.** The playbook ties this stage's health to
  git alone — time from plan approval to merged PR, how often the merged diff still matches
  the committed `plan.md`. Nothing here needs forge to build a dashboard: keeping `Status`
  honest and committing normally is what makes those numbers computable later.
