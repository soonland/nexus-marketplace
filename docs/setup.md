# `/nexus:setup`

One-time setup of a project repo for the nexus plugin. Run it once per repo, from the repo's folder.

## What you'll see
It detects the repo with `gh`, then asks (or proposes a default for) three things:
- **Project board URL**: optional, leave blank to skip.
- **Components**: the parts of your system specs and issues can be scoped to (it proposes a list from your `apps/`, `packages/` or workspace folders; `cross-cutting` is always included).
- **Default assignee**: your GitHub login unless you say otherwise.

## What it changes
- Writes `.claude/nexus.json` (repo, board, components, assignee). If the file exists, it shows it and asks whether to update or keep it.
- Merges the marketplace and `nexus@nexus-marketplace` into `.claude/settings.json`, so teammates are prompted to install them when they trust the folder. Existing keys are kept.
- Creates GitHub labels in the repo: `feature-spec` and one per component, plus the priority labels `P0` to `P3` and `needs-info`.

## What it does not do
- It does not commit or push. Commit `.claude/nexus.json` and `.claude/settings.json` yourself if you want to share them.
- It does not touch any repo but the current one.

## Needs
`gh`, authenticated.

## Undo
Delete the two files and the labels it created. Re-running it is safe.
