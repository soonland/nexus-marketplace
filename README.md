# nexus-marketplace

Personal Claude Code marketplace.

## Install

From GitHub (anyone with access to the repo):

```
/plugin marketplace add soonland/nexus-marketplace
/plugin install nexus@nexus-marketplace
```

To get it for a whole team without everyone typing this, run `nexus:setup` in the project. It adds the marketplace and the plugin to the project's `.claude/settings.json`, and Claude Code prompts teammates to install them when they trust the folder. For a private repo, each person needs GitHub access (`gh auth login` or an SSH key) so Claude Code can clone it.

Update after changes are pushed: `/plugin marketplace update nexus-marketplace`.

### Local development

To test changes before pushing, add the working copy instead:

```
/plugin marketplace add /path/to/nexus-marketplace
/plugin install nexus@nexus-marketplace
```

A local path only works on your machine, so do not use it in a project's shared settings.

## Plugins

| Plugin | What it is |
|---|---|
| `nexus` | Project workflow toolkit: skills for the spec-to-release workflow, plus two agents. |
| `example-plugin` | Minimal template to copy when starting a new plugin. Safe to delete. |

### `nexus`

**Skills** (invoked as `nexus:<skill>`)

| Skill | Purpose |
|---|---|
| `setup` | One-time setup of a project repo: enables the plugin, writes `.claude/nexus.json`, creates labels. |
| `repo-standards` | Audit, set up, or normalize a repo: labels, issue templates, stack-aware CI and Dependabot, Claude workflows, commit tooling, branch protection, and a "Getting started" section in `CLAUDE.md` (run commands and required environment variables). |
| `feature-spec` | Short discovery conversation (one question at a time, playback, builder check) that publishes a feature spec as a GitHub Issue. |
| `plan-issues` | Splits a spec into one implementation issue per package, with dependency order. Also syncs progress and closes the spec when all children are done. |
| `implement-feature` | Implements a spec or an implementation issue from a bare issue number ("do 213"): plan, code, tests, checks, smoke test, PR. Asks "subagent or native?" before executing the plan. |
| `fix` | Root-cause-first bug fix routine. |
| `changelog` | Builds `CHANGELOG.md` from merged PRs and Conventional Commits. |

**Agents**

| Agent | Purpose |
|---|---|
| `mui-layout-designer` | Material UI (v9) layouts, using current APIs looked up through the MUI MCP server. |
| `vitest-unit-tester` | Vitest (v5) unit tests, written for the installed version. |

**MCP server:** the plugin bundles `@mui/mcp` (`.mcp.json`) for the MUI agent. It runs through `npx`, so Node is required.

## Workflow

```
nexus:setup                      once per repo (and nexus:repo-standards for CI, labels, templates)
        │
nexus:feature-spec               publishes "Feature Spec: <name>" with label feature-spec
        │
nexus:plan-issues                one implementation issue per affected package   (skip for single-package specs)
        │
nexus:implement-feature <issue>  branch, plan, code, tests, checks, smoke test, PR
        │                        (asks whether to add the claude-review label)
   PR merged
        │
nexus:plan-issues <spec>         sync mode: ticks merged children, closes the spec when all are done
        │
nexus:changelog                  release notes and tag
```

`nexus:fix` is used whenever a bug or failing build shows up along the way.

Every skill works in the repo of the current working directory (never this one) and uses `.claude/nexus.json` for defaults when it exists:

```json
{
  "repo": "owner/repo",
  "projectBoardUrl": "https://github.com/users/OWNER/projects/N",
  "components": ["web", "api", "cross-cutting"],
  "defaultAssignee": "login"
}
```

All of it is optional. Skills detect the repo with `gh` and ask when something is missing. The `gh` CLI must be installed and authenticated.

## Add a plugin or skill

1. For a new plugin, copy `plugins/example-plugin` to `plugins/<name>`, edit its `.claude-plugin/plugin.json`, and register it in `.claude-plugin/marketplace.json` under `plugins`.
2. Add skills in `skills/<skill>/SKILL.md` and agents in `agents/<agent>.md`. Plugins also support `commands/`, `hooks/hooks.json`, and `.mcp.json`.
3. Keep skills project-neutral: no hard-coded repos, project boards, package managers, or user paths.
4. Bump `version` in `plugin.json` when you publish a change.
5. Validate: `claude plugin validate .`
