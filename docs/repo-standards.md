# `/nexus:repo-standards`

Audit or set up a repo's standard configuration: labels, issue templates, CI, Dependabot, Claude workflows, commit tooling, `CLAUDE.md`, and (optionally) branch protection. Pass `audit`, `setup` or `normalize`, or just describe what you want.

## What you'll see
1. **An audit table**: one row per item, marked ok / missing / differs. If you only asked for an audit, it stops here and changes nothing.
2. **A checklist** of what to apply. Missing items are pre-selected; "differs" items and branch protection are not.
3. A few questions it can't infer: runtime version for CI, coverage threshold, and the model for the Claude workflows.

## What it changes
- **Files** go on a new branch, `chore/repo-standards`, and are committed locally. It does not push or open a PR unless you ask. Existing files are never overwritten without showing a diff.
- CI and Dependabot follow your stack: Node (pnpm, npm, yarn, bun), Python (uv, poetry, pdm, pipenv, hatch, pip), Go, Rust, JVM. It uses the commands your repo already has and drops steps for tools you don't use.
- **Labels and branch protection** change GitHub straight away, so it lists the exact commands and waits for your OK first.

## What it never does
- Read, print or set secrets. For the Claude workflows it tells you to run `claude setup-token` and `gh secret set CLAUDE_CODE_OAUTH_TOKEN` yourself.
- Write to the default branch.

## At the end
A **"Remaining before this is live"** list: push the branch, add the secret, install the GitHub app, and so on.

## Needs
`git` and `gh` with enough rights to edit the repo (labels and protection need admin or maintain).
