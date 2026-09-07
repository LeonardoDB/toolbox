# Issue tracker adapters

Shared fetch reference: how to pull a GitLab or Linear issue's full content, for any skill in
this plugin that needs it — used today by Intent's Mode C, meant to be reused as-is once the
v0.4 PR/MR stage exists. Covers *fetching only*: what a skill does with the pulled issue is
that skill's own instructions, not this file's — keep it that way when extending this file.

Which adapter to use is decided by the URL's host, not by asking the user. If the host
matches no section below, no adapter exists yet — say so plainly and stop; how to proceed
from there is the calling skill's call, not this file's.

Use the adapter below rather than a generic web fetch, tracker or not: it's the only way to
get the full issue — every comment, every field — in one structured call. This matters most
for Linear, whose UI is client-rendered, so a plain fetch returns an empty shell, not the
issue.

Whatever you pull, quote or closely paraphrase the actual text rather than summarizing from
memory — whoever consumes this later should be able to verify the claim against the source.

## GitLab

**Prerequisite:** `glab` CLI installed and authenticated on this machine (`glab auth status`
to check). Forge does not manage GitLab auth itself — that's `glab`'s job.

Fetch the full issue, including every comment, as JSON in one call:

    glab issue view "<url>" --comments -F json

Pull from that JSON: title, description, state, labels, author, assignees, milestone, every
note/comment in order, and any linked issues/MRs.

## Linear

**Prerequisite:** Linear's official remote MCP server connected for this session
(`https://mcp.linear.app/mcp`, OAuth — no API key to manage). If a Linear issue URL comes in
and no Linear tool is available, say so and ask that it be connected via `/mcp`.

Once connected, the tool names may not be loaded into this context yet — find them with:

    ToolSearch("linear issue")

Use whichever tool reads a single issue (by URL or ID; typically returns title, description,
state, labels, assignee) and whichever lists its comments — pull both; a Linear issue's
discussion often carries context the description alone doesn't.
