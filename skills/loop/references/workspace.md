# Workspace: branch before anything else

Read by `loop`'s workspace step, before Intent even starts — the whole trail (`intent.md`,
`spec.md`, `plan.md`, and the code) should land on one branch, not just the final diff.

Never silently operate on whatever branch happens to be checked out. Always confirm before
switching or creating one.

## Nested repos (monorepo-of-repos)

Some checkouts are an umbrella: a scaffolding repo with each real project nested underneath
as its own git repo. Detect this by nested repos existing (`*/.git` under the working tree,
e.g. `source/*/.git`), not by the absence of the umbrella's own `.git` — the umbrella is
itself a git repo too, so a naive check says yes and looks branchable. It isn't: never
branch, switch, or create a worktree in the umbrella. Resolve the actual target repo first,
work inside it, and do all of the below there. A change spanning repos branches in each one —
never once at the umbrella level.

## Steps

1. **Detect git state** in the target repo: current branch (`git branch --show-current`),
   clean or dirty (`git status --porcelain`). If switching would disturb a dirty tree, stop
   and let the user decide (stash/commit) — don't clobber uncommitted work.
2. **Resume or new?** If the checked-out branch already matches the slug this piece of work
   uses, you're set — no switch needed; this step collapses to a one-line confirmation.
   Otherwise, propose a branch (below).
3. **Find the real base — don't hardcode `main`.** `git symbolic-ref refs/remotes/origin/HEAD`
   (or `git remote show origin`) reveals the actual integration branch, which may be `main`,
   `master`, or `develop`. Fetch so it's current, and branch off that — not off whatever's
   currently checked out.
4. **Build the branch name.** Precedence: this project's `.claude/forge.md` **Branching**
   section, if it names one — then a branch-naming rule in the repo's own `CLAUDE.md` (many
   teams already document their convention there, so check before falling back) — otherwise
   the default:
   ```
   <type>/<slug>
   ```
   reusing the slug this piece of work already has (the same one `intent/<slug>.md` uses) —
   one name across the branch and every artifact. `type` is `feat`, `fix`, or `chore`; infer
   it from the work and confirm.
5. **Propose and confirm before switching.** State it plainly — "branch `feat/faster-
   checkout` off `main` (fetched, up to date)?" — and wait. Default is branch-in-place in the
   current checkout, not a worktree: no dependency reinstall, and anything already running
   against this checkout keeps working.
6. **Offer a worktree only when isolation is actually wanted** — the user needs their current
   checkout untouched, or wants this running alongside something else. Name the cost plainly
   if you do: a separate dependency install, and anything bound to the main checkout path
   won't see the worktree's code without reconfiguring it. Don't default to this.

## Output

Report the active branch, what it was cut from, whether it's in-place or a worktree, and any
caveat (a dirty tree handled, dependencies to install). If the checked-out branch was already
right for this work, say so in one line — this is a guard, not ceremony.
