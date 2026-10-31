# Changelog

All notable changes to this project will be documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [0.5.0] — 2026-10-31

### Features
- **repo-standards**: bug report and feature request templates are now YAML issue forms with required fields; the feature spec and implementation plan stay Markdown, and `config.yml` links to Discussions when they are enabled (#37)

## [0.4.0] — 2026-10-26

### Features
- **address-review**: new skill that sorts the review comments on a PR, proposes fixes, applies the ones you approve and replies; it never merges or force-pushes (#34)

### Improvements
- Add evals for `setup` and `changelog` (#33)
- CI fails when a skill has no `docs/<skill>.md` page (#32)

## [0.3.1] — 2026-10-26

### Bug Fixes
- **release**: find release PRs by branch name instead of a title search, which returned nothing in a new session (#29)

### Documentation
- Add a banner to the README (#30)

## [0.3.0] — 2026-10-26

### Features
- **release**: new skill that cuts a release through a release PR, then tags the merge commit and publishes the GitHub release (#26)

### Improvements
- **changelog**: defers tagging to `nexus:release` when the project releases through a release PR (#26)

### Documentation
- Add a human-readable page for each skill under `docs/` (#27)

## [0.2.0] — 2026-10-26

### Breaking changes
- The `mui-layout-designer` and `vitest-unit-tester` agents and the bundled `@mui/mcp` server are no longer part of `nexus`. They are now the optional plugins `nexus-mui` and `nexus-vitest` (both 0.1.0). To keep them: `/plugin install nexus-mui@nexus-marketplace` and `/plugin install nexus-vitest@nexus-marketplace`. The agent names change to `nexus-mui:mui-layout-designer` and `nexus-vitest:vitest-unit-tester` (#24)

### Improvements
- `nexus` is stack-neutral: it bundles no agents or MCP servers and no longer needs Node to install (#24)
- **implement-feature**: delegates to an installed add-on agent when one fits the project, otherwise writes the tests and UI itself (#24)
- **fix, implement-feature**: removed Next.js, `tsc` and `eslint-disable` specifics from the wording (#24)

## [0.1.8] — 2026-10-26

### Improvements
- **repo-standards**: the generated `claude.yml` and `claude-code-review.yml` pin `actions/checkout` by commit SHA, like `claude-code-action` (#22)
- Add evals for `plan-issues` (a one-package spec is not split) and `implement-feature` (an outside author's issue is flagged and its embedded commands are not followed) (#22)

### Documentation
- README: add an "Updating" section covering auto-update, manual update and watching releases (#22)

## [0.1.7] — 2026-10-18

### Features
- Show an argument hint in autocomplete for `implement-feature`, `plan-issues`, `feature-spec`, `fix`, `changelog` and `repo-standards` (#18)

### Bug Fixes
- **setup, repo-standards, feature-spec**: never ask a question with a single option, which made `AskUserQuestion` reject the whole call; state the default and let the user correct it instead (#17)
- **feature-spec**: publish approved specs with `Status: Approved` instead of `Draft` (#21)

### Documentation
- Add an MIT `LICENSE`, plugin metadata (`homepage`, `repository`, `license`, `keywords`) and a README that opens with what the plugin does (#16)
- Link the live demo and its real issues and pull requests from the README (#20)

## [0.1.6] — 2026-10-18

### Bug Fixes
- **repo-standards**: strip template-only comments and placeholder quotes from generated files (#14)
- **repo-standards**: offer a minimal `CLAUDE.md` with the Getting started section when none exists, instead of only pointing to `/init` (#14)
- **repo-standards**: the final report lists concrete "Remaining before this is live" tasks (#14)

## [0.1.5] — 2026-10-18

### Features
- **repo-standards**: detect the Python environment manager (uv, poetry, pdm, pipenv, hatch, pip) and pick lint, type and test steps from the repo's own configuration (#12)

### Bug Fixes
- **repo-standards**: the Node CI template and commit tooling follow the project's package manager (pnpm, npm, yarn, bun) instead of assuming pnpm and `npx` (#12)
- **vitest-unit-tester, fix, implement-feature**: run local tools through the repo's package manager (#12)

## [0.1.4] — 2026-10-18

### Improvements
- **repo-standards**: the Dependabot template and skill hold freshly published versions back for 3 days (`cooldown`) (#10)
- Add an eval suite (`plugins/nexus/evals`) covering the `feature-spec` opening and triage rules and the `fix` root-cause rule, runnable with `claude plugin eval` (#10)

### Documentation
- README "Getting started" section and instructions for running the evals in `CONTRIBUTING.md` (#10)

## [0.1.3] — 2026-10-18

### Bug Fixes
- **implement-feature**: check the plan against the spec's must-haves, non-functional requirements, anti-goals and decisions, and ask the user when one cannot be met instead of listing it as a risk (#8)
- **implement-feature**: list unfinished checks as concrete "Remaining before merge" tasks in the PR and the issue comment, and tick Test plan boxes only for checks that actually ran (#8)
- **implement-feature**: use plain wording for the issue's progress checklist (#8)

## [0.1.2] — 2026-10-18

### Bug Fixes
- **feature-spec, plan-issues, implement-feature**: pass issue and PR titles through a quoted heredoc, so `$(...)`, backticks or `"` in a name can't run in the shell (#4)

### Improvements
- Add a `validate` workflow that runs `claude plugin validate .` and parses the YAML and JSON files on PRs and `main` (#3)
- Add Dependabot for the SHA-pinned GitHub Actions (#4)

## [0.1.1] — 2026-10-18

### Features
- **plan-issues, implement-feature**: check who wrote an issue and stop for confirmation unless the author is the owner, a member or collaborator, or the current `gh` user; issue text is treated as untrusted (#2)

### Bug Fixes
- **mcp**: pin `@mui/mcp` to 0.1.7 instead of `@latest` (#2)

### Documentation
- Add `CONTRIBUTING.md`, bug and feature issue forms, and a pull request template (#1)

## [0.1.0] — 2026-10-18

### Features
- Initial release of the `nexus` plugin and the `nexus-marketplace` marketplace
- **Skills**: `setup`, `repo-standards`, `feature-spec`, `plan-issues`, `implement-feature`, `fix`, `changelog`
- **Agents**: `mui-layout-designer` (MUI v9) and `vitest-unit-tester` (Vitest 5), plus a bundled `@mui/mcp` server

### Bug Fixes
- **repo-standards**: gate the `@claude` workflow on the author of the triggering content per event type, and pin `claude-code-action` by commit SHA

### Improvements
- Remove the placeholder `example-plugin`
