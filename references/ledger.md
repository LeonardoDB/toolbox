# The ledger

A learning noticed mid-flow (a gotcha, a convention, a decision) evaporates by the time
`/forge:run` reports back — the fix is an append-cheap staging file any stage can write to
the moment something worth keeping surfaces, instead of trusting end-of-flow memory to
resurface it.

**Path:** `intent/ledger.md` at the target project's root, unless this project's
`.claude/forge.md` names a different one.

## Appending — free of ceremony

One dated line per entry, an optional rough type tag (`style:`, `gotcha:`, `decision:`,
`pref:`), an optional one-line context. No quality gate at append time — that applies at
promotion, not here. If you hesitated whether it's worth an entry, it was:

```
- 2026-09-08 gotcha: the export endpoint 429s above 50rps — the panel needs to cache.
- 2026-09-08 decision: sessions store refresh tokens in httpOnly cookies, not localStorage.
```

Any stage — Intent, Spec, Plan, the implementer, the verifier — appends here as it happens,
not in a sweep at the end.

## Triage — where the quality gate lives

Triggers: `/forge:run` reaching the end of a run (step 8), or the user asking directly
("triage the ledger"). Walk each entry and either promote it or drop it — most raw entries
don't survive triage, and that's healthy. An entry only gets promoted if **all** of:

1. **Reusable** — a future run of this project is genuinely likely to need it.
2. **Verified** — backed by code, a test, command output, or an explicit user statement, not
   speculation.
3. **Non-obvious** — not trivially visible in the nearest file (exception: a mistake that
   keeps recurring is worth capturing precisely because it keeps recurring).
4. **Safe** — no secrets, credentials, customer PII, or raw logs.

Promote to this project's `CLAUDE.md` (a convention/gotcha Claude should read every session),
this project's `knowledge-base/` if it has one (domain/architecture knowledge — see spec's
SKILL.md), or a target this project's `.claude/forge.md` names under **Ledger** (a specific
wiki page, a style skill's `SKILL.md`). No home fits → drop it. Either way, remove the entry
from the ledger once triaged — it's an inbox, not a landfill; a ledger that only grows has
failed.
