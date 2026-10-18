# Changelog

All notable changes to this project will be documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

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
