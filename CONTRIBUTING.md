# Contributing

Thanks for taking an interest. This is a personal marketplace, so I keep the scope small on purpose. Open an issue before starting anything larger than a fix.

## Before you start

- **Bugs and small fixes:** open a PR directly, or an issue first if you are not sure.
- **New skills or agents:** open a feature request first. A skill has to be useful across projects and stacks, and I may decline ones that are too specific.
- **Questions:** open an issue; there is no other channel.

## What a good change looks like

- **Project-neutral.** No hard-coded repos, project boards, package managers, branch names or user paths. Read them from the repo, `gh`, or `.claude/nexus.json`.
- **Self-contained.** A skill must work with only this plugin and `gh` installed. Do not rely on other plugins.
- **Confirm before outward-facing actions.** Creating issues, PRs, labels, or pushing needs the user's explicit go-ahead.
- **Short and testable.** Prefer a clear step or rule over a paragraph of advice. Say what to run and what to do with the result.
- **Descriptions are triggers.** The `description` in frontmatter decides when Claude picks the skill, so list the phrases that should invoke it.

## Layout

```
.claude-plugin/marketplace.json            marketplace manifest
plugins/nexus/.claude-plugin/plugin.json   plugin manifest (version lives here)
plugins/nexus/skills/<name>/SKILL.md       one skill per folder
plugins/nexus/agents/<name>.md             agents
plugins/nexus/.mcp.json                    bundled MCP servers
```

## Checks

```
claude plugin validate .
```

Then try the change for real: add your working copy with `/plugin marketplace add /path/to/nexus-marketplace`, install `nexus@nexus-marketplace`, and run the skill on a scratch repo.

## Pull requests

- Branch from `main`; `main` only accepts changes through a PR.
- Use [Conventional Commits](https://www.conventionalcommits.org/) for titles (`feat(plan-issues): ...`, `fix(repo-standards): ...`). The changelog is built from them.
- Fill in the PR template. Say what you tested and on what kind of repo.
- Do not bump `version` or edit `CHANGELOG.md` in a feature PR. Both happen in the release PR (see below).
- Anything that adds an MCP server, hook, or shell command is reviewed line by line, since it runs with the user's permissions once installed.

## Releases

A release is one PR, made by the maintainer after feature PRs have merged:

1. Branch from `main` and bump `version` in `plugins/nexus/.claude-plugin/plugin.json`.
2. Add the changelog entry (`nexus:changelog` drafts it from the merged PRs since the last tag).
3. Merge the PR, then tag `v<version>` on `main` and publish the GitHub release.

## Security

Do not open a public issue for a vulnerability, for example a template that lets a stranger trigger a workflow or a skill that runs untrusted input. Use the repository's "Report a vulnerability" button under the Security tab.
