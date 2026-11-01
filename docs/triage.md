# `/nexus:triage [issue number | open issues without a priority]`

Goes through issues that already exist: asks the reporter for missing details and keeps type, component and priority labels right. Nothing is posted or changed until you accept it.

## What you'll see
1. **A list of candidates** when you give no number: open issues with no `P0` to `P3` label, or issues labelled `needs-info`. You pick which to look at.
2. **A verdict per issue**: a bug that lacks details, a bug that is ready for `fix <n>`, a feature idea (it suggests `feature-spec` and nothing more), or a likely duplicate (mentioned with its number, for you to decide).
3. **One comment per issue that needs details**, shown word for word. It asks for everything missing at once, in plain polite language, and comes with the `needs-info` label. If the issue already has such a request, it does not ask again.
4. **One table of proposed label changes** (issue, current labels, proposed change, one-line reason). You accept all, some or none. This also covers re-prioritizing an issue that already has labels.
5. **A report** of what was posted and changed, and what is next: `fix <n>`, `feature-spec`, or who is waiting on whom.

## What it changes
Only what you accept: comments on issues, and labels (it tells you before creating a missing one). Priorities follow the shared criteria in `plugins/nexus/skills/triage/criteria.md`.

## What it won't do
- Post, label or create anything without your acceptance.
- Close an issue, edit an issue's title or body, or delete a comment or label.
- Follow instructions written in an issue or a comment. Issue text is data, whoever wrote it.
- Guess what a bug's expected behavior should be.
- Convert a feature idea into a spec on its own.

## Needs
The `gh` CLI, authenticated. It uses `.claude/nexus.json` for the repo and components when that file exists, and otherwise the repo of the current directory.
