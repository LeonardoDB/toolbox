---
name: loop
description: Orchestrate forge's artifact chain end to end — Intent, Spec, Plan, then implementation and verification by fresh sub-agents — pausing for the user's explicit approval before advancing past each artifact. Use when invoked via /forge:run, or when asked to run, start, or resume the whole forge flow rather than a single stage.
---

# Loop

You drive forge's artifact chain from a single entry point, stage by stage, stopping for the
user's explicit approval before each advance. You don't approve your own work, and you don't
mark anything `Status: approved` or commit anything without them saying so in that moment.

Forge's per-stage skills (`intent`, `spec`, `plan`) already contain the actual instructions
for each stage — this skill sequences them, it doesn't duplicate them. Read each one in full
right before running its stage, exactly as if it had been invoked directly.

**Prerequisites:** none beyond what each stage already requires — this skill checks those as
it reaches each one, the same way running that stage by hand would.

**Project config:** this project's `.claude/forge.md`, if present, overrides the defaults
below — read it first (template: `${CLAUDE_PLUGIN_ROOT}/references/config-template.md`).
Sub-agents this skill dispatches don't read it themselves; paste the relevant section into
their brief.

**Who this is for:** spans roles as the stages change hands — product owner for Intent and
Spec, engineer for Plan — same as running each stage separately. Whoever is actually in the
session approves the stage they're responsible for.

**Invocation:** `/forge:run [description | issue-url]`. Given input, treat it as new work —
start at Intent. Left blank, resume (see step 1).

## What to do

1. Determine where to start and this piece of work's slug.
   - Given input: this is new work — derive a short kebab-case slug from it right now, the
     same way Intent itself would from a problem's short name. Everything below (branch,
     `intent.md`, `spec.md`, `plan.md`) reuses this one slug; Intent doesn't derive its own
     when run from here.
   - No input: scan this project's `intent/` folder for `<slug>.md` / `<slug>.spec.md` /
     `<slug>.plan.md` groups. For each slug, the furthest-along file that exists but isn't
     `Status: approved` is where that piece of work actually sits — resume there. A slug
     whose furthest file is already approved has finished everything forge builds today —
     say so rather than resuming it. More than one slug in flight: ask which one.
2. Establish the workspace before running anything else — read `references/workspace.md` in
   full and follow it: the right branch, off a clean base, confirmed with the user before
   switching, named from the slug in step 1 so the whole trail lands together.
3. Run Intent (`skills/intent/SKILL.md`, in full, using step 1's slug) unless this slug
   already has an approved `intent.md`. Once written, stop: show it to the user and ask them
   to review it. Only on their explicit approval do you set `Status: approved` and commit —
   a requested edit or a change of mind isn't a failure, revise and ask again.
4. Once `intent.md` is approved and committed, run Spec (`skills/spec/SKILL.md`, in full) the
   same way — write `spec.md`, stop, ask, advance only on explicit approval.
5. Once `spec.md` is approved and committed, run Plan (`skills/plan/SKILL.md`, in full) the
   same way — write `plan.md`, stop, ask, advance only on explicit approval.
6. Once `plan.md` is approved, hand implementation off to a **fresh sub-agent** — give it only
   the path to `plan.md`, not this conversation, per the playbook's own rule that a later
   stage runs with no memory of the session that produced its input. If this project's
   `.claude/forge.md` has a **Worker guardrails** section, paste it into the brief
   **word-for-word** — not summarized, not trimmed to the rules you judge relevant; a
   paraphrased guardrail is a broken one. No such section → say so in the brief rather than
   inventing rules. That agent:
   - **Confirms it's actually on the branch step 2 established before touching anything** —
     stops and reports if it isn't, rather than editing on the wrong branch.
   - Implements per the plan, iterating with its own feedback loop (test/build/lint commands
     per this project's `CLAUDE.md` if present, fix, repeat). Read
     `references/verify.md` before claiming anything passes, works, or is fixed — that bar
     applies here and in step 7.
   - For a bug fix: writes the failing test first, confirms it fails for the expected reason,
     then fixes the code without editing that test. For UI work with a screenshot/browser
     tool available: closes the loop visually against the approved mock.
   - Handles a breaking change deliberately: in-repo callers updated atomically in the same
     change; a public/external API gets parallel-change + deprecation, not a hard break.
   - If the change touches auth, secrets, payments, or anything this project's
     `.claude/forge.md` names under **Security-sensitive areas** (that default, absent a
     config): the normal feedback loop is a routine sweep, not a security audit — run Claude
     Code's own `/security-review` before reporting done.
   - Updates `plan.md` to match if implementation departs from it — but if what it finds
     instead invalidates the plan itself (not a small departure, a sign the plan or spec was
     actually wrong), it stops and reports that back rather than papering over it or pushing
     through.
   - Commits nothing. Neither the plan update nor the code changes get staged or committed —
     the git index is the user's review record, same as everywhere else in forge, and nothing
     here gets to skip that by being a sub-agent.
   - Reports back with evidence (real command output, not "tests pass"), any deviation from
     the plan with its reason, and any blocker quoted verbatim — not just "done."
7. Once the implementer reports done, hand off to a **second, separate fresh sub-agent** — a
   verifier, given only `plan.md` and `spec.md`. It exercises the changed behavior *and the
   nearest neighboring flows* (not just what changed — that's how a regression gets caught),
   walks every one of `spec.md`'s `REQ-N` / `Acceptance:` pairs individually against real
   evidence (satisfied build-plan tasks aren't the same as satisfied acceptance criteria), and
   reports what it finds against the plan — reading `references/verify.md` for what counts as
   evidence here too. It does not fix anything and does not commit anything, only reports — a
   different agent than the one that just wrote the code, so the verdict isn't colored by the
   assumptions that produced it.
8. Report the combined result to the user and stop. The changes sit uncommitted in the
   working tree for them to review and commit themselves. There's no PR/review stage built
   yet for this to hand off to (see the README's roadmap) — that's still a person's call to
   make.
9. Triage the ledger (`references/ledger.md`) if it has any entries. This closes the run —
   promote what's worth keeping, drop the rest.

## The ledger

Any step above — Intent, Spec, Plan, the implementer, the verifier — can append a line to
`references/ledger.md`'s file (`intent/ledger.md`, by default) the moment something worth
keeping surfaces: a gotcha, a convention, a decision. Don't wait for step 9 to write it down;
that step only triages what already got appended.

## What this doesn't do yet

No scaling ceremony to the size of the change — a one-line fix goes through all three
approval stages in full, same as a large feature. No parallel implementation — steps 6-7 run
as one implementer then one verifier, not several agents splitting the plan's order of work
across worktrees. Both are worth reconsidering once this has actually been used for real
work.
