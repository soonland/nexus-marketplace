# `/nexus:plan-issues <spec issue>`

Splits an approved feature spec into **one implementation issue per affected package**, in dependency order. Run it again later to sync progress.

## What you'll see
1. It reads the spec and **checks who wrote it**. If the author isn't the owner or a collaborator, it shows you the issue and waits for your go-ahead.
2. It works out the affected packages from the spec, `.claude/nexus.json` and the repo structure, and the dependencies from the code (manifests and imports).
3. **The whole plan, before anything is created**: issue titles, tasks, what depends on what, what can run in parallel, and risks.

If the spec touches only one package, it says so and offers to implement it directly instead of splitting.

## What it changes
Only after you confirm:
- creates the issues (`implementation` label plus a package label), blockers first;
- links them to the spec as GitHub sub-issues, or with a task-list comment if that isn't available;
- adds them to the project board if one is configured.

## Sync mode
If the spec already has children, you can **sync**: it ticks finished ones, reports what is unblocked, and, when every child is closed, asks you to confirm the success metrics before closing the spec. Nothing changes without your confirmation.

## What it never does
Write code or open PRs.

## Needs
`gh`, authenticated.
