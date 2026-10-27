# nexus: from idea to merged PR, on GitHub

[![validate](https://github.com/soonland/nexus-marketplace/actions/workflows/validate.yml/badge.svg)](https://github.com/soonland/nexus-marketplace/actions/workflows/validate.yml)
[![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

A [Claude Code](https://claude.com/claude-code) plugin that turns a rough idea into a reviewed spec, one issue per package in dependency order, and a pull request that lists what is still left to check. Everything lands as ordinary GitHub issues and PRs, so your team sees and reviews it the usual way.

- **Specs you can trust.** `feature-spec` asks one question at a time and keeps what *you* decided apart from what *it* assumed.
- **Work split by package.** `plan-issues` creates one issue per affected package, in dependency order, linked to the spec.
- **PRs that say what is left.** `implement-feature` plans, writes code and tests, and opens a PR with a "Remaining before merge" checklist instead of a vague caveat.
- **Safe by default.** Issue text from strangers is treated as data, not instructions; generated CI follows your package manager (pnpm, npm, yarn, bun) and Python tooling (uv, poetry, pip and more).

**[Live demo](https://soonland.github.io/nexus-demo/)**: a tiny project built end to end with the plugin (`setup`, `repo-standards`, `feature-spec`, `implement-feature`). Its [issues and pull requests](https://github.com/soonland/nexus-demo) are the real, unedited output.

Splitting a spec across several packages (`plan-issues`) is shown in a larger project: a [feature spec](https://github.com/soonland/nexus-workflow/issues/500) split into four package issues ([#501](https://github.com/soonland/nexus-workflow/issues/501), [#502](https://github.com/soonland/nexus-workflow/issues/502), [#503](https://github.com/soonland/nexus-workflow/issues/503), [#504](https://github.com/soonland/nexus-workflow/issues/504)), and a [PR](https://github.com/soonland/nexus-workflow/pull/499) it opened.

Releases are listed in [CHANGELOG.md](CHANGELOG.md).

## Install

From GitHub:

```
/plugin marketplace add soonland/nexus-marketplace
/plugin install nexus@nexus-marketplace
```

To get it for a whole team without everyone typing this, run `nexus:setup` in the project. It adds the marketplace and the plugin to the project's `.claude/settings.json`, and Claude Code prompts teammates to install them when they trust the folder. For a private repo, each person needs GitHub access (`gh auth login` or an SSH key) so Claude Code can clone it.

### Updating

Third-party marketplaces do not update themselves by default. Either:

- turn on auto-update: `/plugin` → **Marketplaces** → `nexus-marketplace` → **Enable auto-update**; or
- update by hand: `/plugin marketplace update nexus-marketplace` (or `claude plugin update nexus@nexus-marketplace` from a shell).

To hear about new versions, use **Watch → Custom → Releases** on this repo. Each release lists what changed (see also [CHANGELOG.md](CHANGELOG.md)).

### Local development

To test changes before pushing, add the working copy instead:

```
/plugin marketplace add /path/to/nexus-marketplace
/plugin install nexus@nexus-marketplace
```

A local path only works on your machine, so do not use it in a project's shared settings.

## Getting started

In the project repo you want to work on (the skills act on the repo of the folder Claude Code runs in):

1. `/nexus:setup` once per repo. It writes `.claude/nexus.json`, creates the labels, and offers to enable the plugin for teammates.
2. `/nexus:feature-spec` and describe what you want. It asks one question at a time and publishes the spec as a GitHub Issue after you approve it.
3. `/nexus:plan-issues <spec number>` only if the spec touches several packages. It creates one issue per package, in dependency order.
4. Say "do 123" (or `/nexus:implement-feature 123`) for an issue. It plans, asks whether to use subagents, writes code and tests, and opens a PR with a list of anything still to check before merging.
5. After a PR merges, run `/nexus:plan-issues <spec number>` again to tick finished issues and close the spec. `/nexus:release` cuts the release (see below).

Optional: `/nexus:repo-standards` audits or sets up labels, issue templates, CI, Dependabot, commit tooling and a "Getting started" section in the project's `CLAUDE.md`.

## Plugins

| Plugin | What it is |
|---|---|
| `nexus` | Project workflow toolkit for any stack: skills for the spec-to-release workflow. |
| `nexus-mui` | Optional add-on: Material UI layout agent with the MUI MCP server (needs Node). |
| `nexus-vitest` | Optional add-on: Vitest unit-test agent. |

### `nexus`

**Skills** (invoked as `nexus:<skill>`). Each links to a short page on what it asks, what it changes, and what it never does without your OK.

| Skill | Purpose |
|---|---|
| [`setup`](docs/setup.md) | One-time setup of a project repo: enables the plugin, writes `.claude/nexus.json`, creates labels. |
| [`repo-standards`](docs/repo-standards.md) | Audit, set up, or normalize a repo: labels, issue templates, stack-aware CI and Dependabot, Claude workflows, commit tooling, branch protection, and a "Getting started" section in `CLAUDE.md` (run commands and required environment variables). |
| [`feature-spec`](docs/feature-spec.md) | Short discovery conversation (one question at a time, playback, builder check) that publishes a feature spec as a GitHub Issue. |
| [`plan-issues`](docs/plan-issues.md) | Splits a spec into one implementation issue per package, with dependency order. Also syncs progress and closes the spec when all children are done. |
| [`implement-feature`](docs/implement-feature.md) | Implements a spec or an implementation issue from a bare issue number ("do 213"): plan, code, tests, checks, smoke test, PR. Asks "subagent or native?" before executing the plan. |
| [`fix`](docs/fix.md) | Root-cause-first bug fix routine. |
| [`changelog`](docs/changelog.md) | Builds `CHANGELOG.md` from merged PRs and Conventional Commits. |
| [`release`](docs/release.md) | Releases through a release PR: picks the version, bumps it, writes the changelog entry and opens the PR; after it merges, tags and publishes the GitHub release. Does not publish to package registries. |

`nexus` works with any language or framework and bundles no agents or MCP servers. When an add-on below is installed, `implement-feature` delegates to it where it fits; without it, the skill writes the tests and UI itself.

### `nexus-mui` (optional)

`mui-layout-designer`: Material UI (v9) layouts, using current APIs looked up through the bundled `@mui/mcp` server. The server runs through `npx`, so Node is required. Its version is pinned in `plugins/nexus-mui/.mcp.json` on purpose (an unpinned `@latest` would run whatever is published); bump it deliberately.

```
/plugin install nexus-mui@nexus-marketplace
```

### `nexus-vitest` (optional)

`vitest-unit-tester`: Vitest (v5) unit tests, written for the installed version.

```
/plugin install nexus-vitest@nexus-marketplace
```

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
nexus:release                    release PR (version + changelog); after merge: tag and GitHub release
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

1. For a new plugin, create `plugins/<name>/.claude-plugin/plugin.json` (`name`, `version`, `description`, `author`) and register it in `.claude-plugin/marketplace.json` under `plugins` with `name`, `source` (`./plugins/<name>`) and `description`.
2. Add skills in `skills/<skill>/SKILL.md` and agents in `agents/<agent>.md`. Plugins also support `commands/`, `hooks/hooks.json`, and `.mcp.json`.
3. Keep skills project-neutral: no hard-coded repos, project boards, package managers, or user paths.
4. Bump `version` in `plugin.json` and add a `CHANGELOG.md` entry in a release PR, not in the feature PR (see `CONTRIBUTING.md`).
5. Validate: `claude plugin validate .`
