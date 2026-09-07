# Evidence-based completion

Used by the implementer and verifier sub-agents (`loop`'s steps 5-6) whenever either is about
to claim something works, passes, builds, or is fixed. The evidence comes from the toolchain,
not from the agent's own confidence — the same discipline the playbook calls for at Stage 4.

## The rule

Don't claim completion without fresh command output from this run. Cached output, "I ran
this earlier," and "it should work" are all disqualified — state changes; only the command
you just ran counts.

Red flags in your own claim — if you're about to write one of these, stop and run the command
instead: "should", "should be", "probably", "seems to", "looks like it", "I believe", "this
will fix it."

## The gate

1. Identify the exact command that proves this specific claim.
2. Run it in full — no partial runs, no "I'll just check the relevant test" when the claim is
   about the whole suite.
3. Read the complete output; check the exit code; count passes/failures, don't infer them
   from the absence of red text.
4. Confirm the output proves the *exact* claim — "it compiled" doesn't prove "the feature
   works."
5. Report the result with the command, exit code, and the key output line, not just a
   conclusion.

## Claim → evidence

| Claim | What proves it |
| --- | --- |
| Tests pass | Command run, exit 0, explicit pass count, 0 failures |
| Build succeeds | Build command run, exit 0 |
| Lint clean | Linter run, 0 errors |
| Bug is fixed | Reproduced first (must fail), fix applied, reproduced again (now passes) |
| A requirement is met | The spec.md `Acceptance:` clause for that `REQ-N` checked
individually against real evidence — not the plan's tasks treated as a bundle |
| No regressions | The affected suite run before the change and after — zero *new*
failures; "pre-existing" is a claim to check, not assume |

## Rationalizations to reject

"It's a trivial change" (trivial changes break builds too). "I ran something like it
earlier" (earlier isn't now). "It compiles, so it works" (compilation isn't behavior). "The
sub-agent said it was done" (require the evidence in its report, not the claim).

## When it fails

Report the failure honestly with the output — a caught failure is the gate working, not a
setback. Fix and re-run. Cap this fix-verify cycle at two rounds; if it still isn't passing
after that, stop and surface the open list to the user instead of looping — that's usually a
sign the plan or spec was wrong, not that one more attempt will close it.
