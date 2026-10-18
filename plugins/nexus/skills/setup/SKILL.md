---
name: setup
description: Initialize the current project repo for the nexus plugins. Use when the user asks to "set up", "init", "configure" or "onboard" this project for the nexus plugins, or when a nexus plugin reports that `.claude/nexus.json` is missing.
---

# Soonland Setup Skill

One-time (re-runnable) setup of the developer's project repo. It:
1. Registers the nexus plugins and enables its plugins in the project's `.claude/settings.json`.
2. Writes project defaults to `.claude/nexus.json` so other nexus plugins stop asking the same questions.
3. Creates the GitHub labels the plugins rely on.

Everything happens in the **developer's project repo** (the current working directory), never in the marketplace repo.

Requires the `gh` CLI, authenticated.

---

## Protocol

Use `AskUserQuestion` conversationally. **`AskUserQuestion` needs 2 to 4 distinct options per question** (a free-text "Other" is added for you), and one question with a single option makes the whole call fail. For a value that has one sensible default or needs free text (a URL, a username, a version), do not wrap it in a one-option question: state the default you will use and let the user correct it in plain text, or combine it with real alternatives in one question (for example "Use <default>" / "Leave blank"). Be idempotent: read existing files first and offer to keep or update existing values instead of overwriting them.

### Step 1 — Detect the project

```bash
git rev-parse --show-toplevel
gh repo view --json nameWithOwner --jq '.nameWithOwner'
gh api user --jq '.login'
```

If there is no git repo or GitHub remote, ask the user for `owner/repo`. Confirm the detected repo with the user.

If `.claude/nexus.json` already exists, show its contents and ask whether to update or keep it.

### Step 2 — Collect defaults

Ask:
1. **Project board URL** (optional, e.g. `https://github.com/users/OWNER/projects/N` or `https://github.com/orgs/ORG/projects/N`). Blank to skip.
2. **Components** — the parts of the system specs and issues can be scoped to. Propose a list inferred from the repo (top-level `apps/`, `packages/`, `services/` directories, or workspace entries) and let the user edit it. `cross-cutting` is always included.
3. **Default assignee** — default to the authenticated GitHub user.

### Step 3 — Write the project config

Write `.claude/nexus.json` at the repo root:

```json
{
  "repo": "owner/repo",
  "projectBoardUrl": "https://github.com/users/OWNER/projects/N",
  "components": ["web", "api", "cross-cutting"],
  "defaultAssignee": "login"
}
```

Omit `projectBoardUrl` if skipped. Preserve any other keys already in the file.

### Step 4 — Enable the plugins

Find where the marketplace was installed from:

```bash
claude plugin marketplace list
```

Merge the following into the project's `.claude/settings.json` (create it if missing, never drop existing keys), so teammates are prompted to install the marketplace and plugins when they trust the folder:

```json
{
  "extraKnownMarketplaces": {
    "nexus-marketplace": {
      "source": { "source": "github", "repo": "<marketplace owner/repo>" }
    }
  },
  "enabledPlugins": {
    "nexus@nexus-marketplace": true
  }
}
```

Use the marketplace's real source from the list output. If it was added from a local path, use `{ "source": "directory", "path": "<path>" }` and warn that this will not work for teammates until the marketplace is pushed to GitHub. Enable `nexus`; ask whether to enable any other plugins in the marketplace.

### Step 5 — Create labels

Create these labels in the project repo if missing (`gh label list --repo "$REPO"` first):

| Label | Color |
|---|---|
| `feature-spec` | `#0075ca` |
| one per component in the config | `#ededed` |

```bash
gh label create "<name>" --repo "$REPO" --color "<hex without #>"
```

### Step 6 — Report

Summarise what was created or changed (files written, labels created, plugins enabled) and remind the user to commit `.claude/nexus.json` and `.claude/settings.json` if they want the setup shared with the team.

---

## Rules

- Never overwrite existing config values without showing them and getting confirmation.
- Never touch repos other than the developer's project repo.
- Always merge into `.claude/settings.json`, never replace it.
- Do not commit or push anything; leave that to the user.
