# forge

Agent-driven SDLC loop for Claude Code, modeled on Anthropic's
[AI-Native SDLC playbook](https://claude.com/blog/the-ai-native-sdlc-playbook). Works across
GitHub, GitLab, and Linear.

## What it does

Walks a change through your local dev environment — plan, design, build, test — as a chain
of committed files, not a conversation you have to remember:

- **`intent.md`** — what's broken or wanted, and why. Written from a conversation, an issue
  link, or log evidence.
- **`spec.md`** — turns an approved intent into requirements and a design, constrained by
  this project's own policies.
- **`plan.md`** — turns an approved spec into an implementation plan, in Claude Code's own
  Plan Mode.
- From there, one agent implements the plan and a second, separate agent verifies the
  result — then everything stops. Nothing is committed or pushed automatically; the changes
  sit in your working tree until you decide.

Run one stage at a time (`/forge:intent`, `/forge:spec`, `/forge:plan`), or the whole chain
at once with `/forge:run`.

## Install

```
/plugin marketplace add <github-user>/forge
/plugin install forge@forge
```

Needs a `.claude-plugin/marketplace.json` once the repo is public.

## License

MIT — see [LICENSE](LICENSE).
