---
name: repo-standards
description: Audit, set up, or normalize a GitHub repository against the owner's standard configuration (labels, issue templates, stack-aware CI and Dependabot, Claude Code workflows, branch protection, commit tooling, CLAUDE.md). Use when the user asks to "set up this repo", "normalize the repo", "apply my usual config", "add CI / dependabot", or "audit repo config".
argument-hint: "[audit | setup | normalize]"
---

# Repo Standards Skill

Bring the **developer's current repository** in line with a standard configuration. It works in three modes:

- **Audit** (default when the user says "check", "audit" or "compare"): read-only report of what is present, missing, or different. Changes nothing.
- **Setup** (new or mostly empty repo): offer the whole standard set.
- **Normalize** (existing repo): fix only the differences the user picks, never overwriting without showing the diff.

Requires `git` and the `gh` CLI, authenticated with enough permission to edit the repo (labels and branch protection need admin or maintain). Work happens in the current working directory's repo, never in the repository this plugin is distributed from.

Templates live in the `templates/` folder next to this `SKILL.md` (use this skill's base directory to locate them). They contain `{{PLACEHOLDERS}}` that you fill in; never copy them blindly. Template files are stored without the leading dot: `templates/github/` becomes `.github/` and `commitlint.config.js` stays as is.

---

## What the standard set contains

| # | Item | Template / action |
|---|---|---|
| 1 | **Labels**: `feature-spec`, `implementation`, `dependencies`, `claude-review`, plus component labels from `.claude/nexus.json` | `gh label create` |
| 2 | **Issue templates**: feature spec, implementation plan, bug report, `config.yml` | `templates/github/ISSUE_TEMPLATE/` |
| 3 | **CI** workflow for the detected stack: install, lint, format check, typecheck, test (with coverage where supported), build | `templates/ci/<stack>.yml` |
| 4 | **Claude Code** workflows: `@claude` on issues and PRs (owner only) and label-gated PR review | `templates/github/workflows/claude.yml`, `claude-code-review.yml` |
| 5 | **Dependabot**: weekly updates for every detected ecosystem plus GitHub Actions, patch bumps ignored, new releases held back 3 days (`cooldown`) | `templates/github/dependabot.yml` |
| 6 | **Commit tooling** (Conventional Commits enforced at commit time): commitlint + husky + lint-staged for Node, the `pre-commit` framework for everything else | `templates/commitlint.config.js` or `templates/pre-commit-config.yaml` |
| 7 | **CLAUDE.md** with a "Getting started" section (how to install, run, test, build, and which environment variables are needed) | see below |
| 8 | **Branch protection** (optional, never pre-selected): require PR + passing CI on the default branch | `gh api` ruleset |

Items 1, 2, 4, 7 and 8 do not depend on the language. Items 3, 5 and 6 adapt to the detected stack (see "Stack detection" below).

---

## Stack detection

Detect the stack from manifest files at the repo root and in workspace directories. A repo can have several (for example a Python API with a Node frontend); handle each one.

| Manifest | Stack | CI template | Dependabot ecosystem |
|---|---|---|---|
| `package.json` | Node | `templates/ci/node.yml` | `npm` |
| `pyproject.toml`, `requirements*.txt`, `Pipfile` | Python | `templates/ci/python.yml` | `pip` (`uv` if `uv.lock` exists) |
| `go.mod` | Go | `templates/ci/go.yml` | `gomod` |
| `Cargo.toml` | Rust | `templates/ci/rust.yml` | `cargo` |
| `pom.xml` | Java (Maven) | `templates/ci/jvm.yml` | `maven` |
| `build.gradle`, `build.gradle.kts` | Java/Kotlin (Gradle) | `templates/ci/jvm.yml` (Gradle variant) | `gradle` |
| `Gemfile` | Ruby | derive from the repo | `bundler` |
| `composer.json` | PHP | derive from the repo | `composer` |
| `*.csproj`, `*.sln` | .NET | derive from the repo | `nuget` |
| `Dockerfile` | Docker | none | `docker` |
| `docker-compose*.yml` | Docker Compose | none | `docker-compose` |
| `*.tf` | Terraform | none | `terraform` |

- **Node package manager:** pick it from the `packageManager` field in `package.json`, else from the lockfile (`pnpm-lock.yaml` pnpm, `package-lock.json` npm, `yarn.lock` yarn, `bun.lock` or `bun.lockb` bun), else ask. In a workspace, use the root's. Fill the Node CI placeholders from this table:

  | Manager | `{{PM_INSTALL}}` | `{{PM_RUN}}` | `{{PM_EXEC}}` | `{{PM_CACHE}}` | Setup step |
  |---|---|---|---|---|---|
  | pnpm | `pnpm install --frozen-lockfile` | `pnpm run` | `pnpm exec` | `pnpm` | keep `pnpm/action-setup` |
  | npm | `npm ci` | `npm run` | `npx` | `npm` | delete `pnpm/action-setup` |
  | yarn (classic) | `yarn install --frozen-lockfile` | `yarn run` | `yarn` | `yarn` | delete `pnpm/action-setup` |
  | yarn (berry, `.yarnrc.yml`) | `yarn install --immutable` | `yarn run` | `yarn` | `yarn` | delete `pnpm/action-setup`; add `corepack enable` before install |
  | bun | `bun install --frozen-lockfile` | `bun run` | `bunx` | none (remove the `cache:` line) | replace `actions/setup-node` with `oven-sh/setup-bun` |

  Use the same "exec" form wherever a command runs a local tool (husky, commitlint, lint-staged, prettier).
- **Python tooling:** work out the environment manager first, then fill the Python CI placeholders. Detect it in this order, from the repo root (the workspace member's directory if it is a monorepo):

  | Tool | Detect by | Setup step to add | `{{PY_INSTALL}}` | `{{PY_RUN}}` | `{{PY_CACHE}}` |
  |---|---|---|---|---|---|
  | uv | `uv.lock` | `astral-sh/setup-uv` (resolve its latest tag with `gh api repos/astral-sh/setup-uv/releases/latest --jq .tag_name`; set `enable-cache: true`) | `uv sync --locked --all-extras --dev` | `uv run ` | remove the `cache:` line |
  | poetry | `poetry.lock` or `[tool.poetry]` | `pipx install poetry` before `setup-python` | `poetry install --no-interaction` | `poetry run ` | `poetry` |
  | pdm | `pdm.lock` or `[tool.pdm]` | `pdm-project/setup-pdm` (resolve its latest tag the same way; `cache: true`) | `pdm install` | `pdm run ` | remove the `cache:` line |
  | pipenv | `Pipfile.lock` or `Pipfile` | `pip install pipenv` | `pipenv install --dev --deploy` | `pipenv run ` | `pipenv` |
  | hatch | `[tool.hatch.envs]` | `pip install hatch` | none (delete the install step) | `hatch run ` | `pip` |
  | pip | `requirements*.txt`, or a `pyproject.toml` with no tool above | none | `pip install -r requirements.txt`, or `pip install -e ".[dev]"` if `pyproject.toml` declares a `dev` extra | empty | `pip` |

  Then pick the steps from what the repo configures, not from the template: if `tox.ini`, `noxfile.py` or a `Makefile` already defines lint, type and test targets, call those instead of the individual tools. Otherwise read `pyproject.toml` and the config files: ruff (`[tool.ruff]`, `ruff.toml`), black, isort, flake8 or pylint (their config or dev dependencies) for lint and format; mypy (`[tool.mypy]`, `mypy.ini`) or pyright (`pyrightconfig.json`) for types; pytest (config or dependency), else `python -m unittest`, for tests. Keep a build step only for a package the project publishes (a `[build-system]` table). Take the Python version from `.python-version`, `requires-python` or the tool's own pin, and the coverage threshold from the repo's config when it has one.
- **CI:** use the matching template as a starting point. The templates are examples of the usual steps, not gospel: replace each command with the one the repo actually uses (read `Makefile`, `justfile`, `tox.ini`, `package.json` scripts, `CONTRIBUTING.md` and `CLAUDE.md`) and delete steps the repo has no tool for. For a stack with no template, write a workflow from the repo's own build and test commands using the same `on:` triggers and structure. Never invent a tool the repo does not use.
- **Dependabot:** repeat the entry in `templates/github/dependabot.yml` once per ecosystem and per directory that holds a manifest (monorepo workspaces get their own entry and label). For `npm`, add `groups: dev-dependencies: dependency-type: development`. The `github-actions` entry is always kept, even when no other ecosystem is detected. Keep the `cooldown` block on every entry: it delays proposing a freshly published version, which is the cheap, stack-independent defence against a compromised release. It only covers Dependabot PRs; a hand-made bump in a PR is not checked.
- **Commit tooling:** if a Node project, use commitlint + husky + lint-staged. For any other stack, use the `pre-commit` framework instead (see Step 4). A repo with both may use either; prefer husky when `package.json` is at the root.

---

## Protocol

### Step 1 — Detect

```bash
REPO=$(gh repo view --json nameWithOwner --jq '.nameWithOwner')
DEFAULT=$(gh repo view --json defaultBranchRef --jq '.defaultBranchRef.name')
gh label list --limit 200 --json name,color
gh secret list
ls -A .github .github/workflows .github/ISSUE_TEMPLATE 2>/dev/null
```

Also detect the stack(s) from the manifests above, and read what each one tells you: for Node, `package.json` (scripts `lint`, `typecheck`, `test`, `test:coverage`, `build`; `packageManager`; `engines.node`; existing `lint-staged`), the lockfile name (to choose pnpm, npm or yarn) and whether Prettier is used; for other stacks, the version pins (`.python-version`, `go.mod`, `rust-toolchain.toml`, the Java version in the build file), the test and lint tools in the project config, and any `Makefile` or task runner. Check for workspaces (monorepo?) and read `.claude/nexus.json` if it exists (components, default assignee).

If there is no GitHub remote, stop and tell the user (offer to create the repo with `gh repo create` only if they ask).

### Step 2 — Audit report

Show one table, one row per item above, with a status of **ok**, **missing**, or **differs** and a one-line note (for example "ci.yml exists but has no coverage step"). For files that exist, compare against the template and summarize the differences in plain words.

If the user asked only for an audit, stop here.

### Step 3 — Choose

Use `AskUserQuestion` (multi-select) to let the user pick which missing or differing items to apply. Pre-select everything marked missing; leave "differs" items and item 8 unselected. **`AskUserQuestion` needs 2 to 4 distinct options per question** (a free-text "Other" is added for you), and one question with a single option makes the whole call fail. For a value that has one sensible default or needs free text (a URL, a username, a version), do not wrap it in a one-option question: state the default you will use and let the user correct it in plain text, or combine it with real alternatives in one question (for example "Use <default>" / "Leave blank"). Then ask for the few values you cannot infer:
- Runtime version for CI (Node, Python, Java; default: the version the repo already pins, else the current LTS or stable release)
- Coverage threshold, where the stack supports one (default: the project's existing one, else 75%)
- For Claude workflows: the model (default `claude-sonnet-5-5`); access stays restricted to the repo owner

### Step 4 — Apply

**Files** (items 2 to 7): create a branch and make the changes there, never directly on the default branch.

```bash
git checkout -b chore/repo-standards
```

For each selected file:
1. Read the template and fill the placeholders (`{{DEFAULT_BRANCH}}`, `{{NODE_VERSION}}`, `{{PYTHON_VERSION}}`, `{{JAVA_VERSION}}`, `{{COVERAGE_THRESHOLD}}`, `{{PM_INSTALL}}`, `{{PM_RUN}}`, `{{PM_EXEC}}`, `{{PM_CACHE}}`, `{{PY_INSTALL}}`, `{{PY_RUN}}`, `{{PY_CACHE}}`, `{{CLAUDE_MODEL}}`, `{{COMPONENT_LABELS}}`, `{{CLAUDE_ACTION_SHA}}`, `{{ECOSYSTEM}}`, `{{DIRECTORY}}`).
2. Adapt to the repo, as described under "Stack detection": use the repo's real commands, remove steps it has no tool for, switch package manager and caching to match, and add `working-directory` or `--filter` handling for monorepos or multi-stack repos (one job per stack).
3. If the target file already exists, show a diff and ask before replacing or merging. Never silently overwrite.
4. For `uses:` lines, resolve the latest major of each action (`gh api repos/<owner>/<action>/releases/latest --jq .tag_name`) instead of trusting the version in the template.
5. **Write clean output.** Remove template-only text from the file you write: comments that tell you how to fill, repeat or adapt the template (anything that mentions `SKILL.md`, "repeat this entry", "remove any step…") and the quotes around a filled placeholder that existed only to keep the template valid YAML (`run: 'npm ci'` becomes `run: npm ci`, unless the value needs quoting). Keep comments that explain why a setting is there, such as the cooldown or the owner gate.
6. **Pin third-party actions that receive secrets to a full commit SHA**, with the version in a trailing comment. This applies to `anthropics/claude-code-action` (placeholder `{{CLAUDE_ACTION_SHA}}`) and any other non-GitHub-owned action that gets a token or secret. Resolve it with `gh api repos/<owner>/<action>/commits/<tag> --jq .sha` (for example tag `v1`), and write `uses: <owner>/<action>@<40-char-sha> # v1`. The Dependabot `github-actions` entry then keeps the SHA up to date. Major tags are acceptable for GitHub-owned `actions/*`.

Item-specific notes:
- **Claude workflows**: the `@claude` trigger must check the association (`OWNER`) of the author of the *triggering content* for each event type (comment, review comment, review, issue). Never gate on `github.event.issue.author_association` for comment events: it describes whoever opened the thread, so any commenter on an owner-authored issue or PR would pass. When auditing an existing `claude.yml`, flag that pattern as **differs** with a security note. They also need the `CLAUDE_CODE_OAUTH_TOKEN` repository secret. Check `gh secret list`. Never set it yourself and never ask the user to paste it into chat. Tell them to run `claude setup-token` in a terminal and then `gh secret set CLAUDE_CODE_OAUTH_TOKEN`. Also mention that the Claude GitHub app must be installed on the repo (`/install-github-app` inside Claude Code).
- **Dependabot**: build the `updates:` list from the detected ecosystems and directories; always keep the `github-actions` entry.
- **Commit tooling, non-Node stacks**: write `.pre-commit-config.yaml` from `templates/pre-commit-config.yaml`, resolving the latest tags with `gh api repos/compilerla/conventional-pre-commit/releases/latest --jq .tag_name` and `gh api repos/pre-commit/pre-commit-hooks/releases/latest --jq .tag_name`. Add the repo's own formatter and linter hooks. Tell the user to install the tool (`pipx install pre-commit` or `brew install pre-commit`), then run:
  ```bash
  pre-commit install --hook-type pre-commit --hook-type commit-msg
  ```
- **Commit tooling, Node**: add dev dependencies with the repo's package manager (`@commitlint/cli @commitlint/config-conventional husky lint-staged`), add `"prepare": "husky"`, and a `lint-staged` block if missing that runs the repo's own formatter and linter on staged files (for example Prettier on `*.{ts,tsx,json,css,md}` and ESLint `--fix` on `*.{ts,tsx}`, only for tools the repo actually uses). Then create the hooks:
  ```bash
  <exec> husky init
  printf '<exec> commitlint --edit $1\n' > .husky/commit-msg
  printf '<exec> lint-staged\n' > .husky/pre-commit
  ```
  Replace `<exec>` with the package manager's runner from the table above (`pnpm exec`, `npx --no --` for npm, `yarn`, `bunx`).
- **CLAUDE.md**: the goal is that nobody (including the owner, months later) has to remember how to start the project or which variables it needs.
  - If there is no `CLAUDE.md`, offer two options: create a minimal `CLAUDE.md` now that contains only the `## Getting started` section described below (built from the repo's real files), or leave it for the built-in `/init`, which analyses the code in more depth and can be run afterwards. Do not write a long CLAUDE.md yourself.
  - If it exists but has no run/setup section, offer to add a `## Getting started` section built **from the repo's real files, never from guesses**: the install, dev, test, lint and build commands (from `package.json` scripts, `Makefile`, `justfile`, or the CI workflow); the required runtime version; services it needs (database, Docker compose, other apps in the stack) and how to start them; and the **environment variables** it expects. For variables, read `.env.example` / `.env.template`, config or schema files that validate env (for example a zod `env.ts`), and `docker-compose.yml`; list each name with a one-line purpose and whether it is required, and never write secret values. If a variable is read in code but absent from the example file, list it and suggest adding it to `.env.example`.
  - Show the generated section and get confirmation before writing it into `CLAUDE.md`. Keep it short (a command table and a variables table are enough), and leave any existing content untouched.
  - If nothing in the repo reveals how to run something, say so and ask instead of inventing a command.
- Run the repo's own checks (lint, typecheck, format check) on the generated files where possible, and fix anything you introduced.

**Labels and branch protection** (items 1 and 8) change GitHub state directly and are outward-facing, so list the exact commands and get confirmation first.

Labels:
```bash
gh label create "<name>" --repo "$REPO" --color "<hex>" --description "<text>"   # skip labels that exist
```
Label set: `feature-spec` (`0075ca`), `implementation` (`0e8a16`), `dependencies` (`0366d6`), `claude-review` (`5319e7`), one per component in `.claude/nexus.json` (`ededed`). Do not delete or rename existing labels.

Branch protection, only if the user explicitly selected it: require a pull request and the CI job to pass on `$DEFAULT`. Confirm the exact required check name from the CI workflow first, and warn that it can lock the user out of direct pushes.

### Step 5 — Commit and report

Commit the file changes with Conventional Commits (`chore: add repo standards`, split by concern if large), ending the message with whatever attribution lines your environment instructs, if any. Do not push or open a PR unless the user asks; offer to.

Report: what was created, what was changed, and what was skipped and why. Then list **Remaining before this is live** as concrete tasks, each saying what to do and where, for example: push the `chore/repo-standards` branch and open the PR (offer to do it), add the `CLAUDE_CODE_OAUTH_TOKEN` secret, install the GitHub app, run `/init` if `CLAUDE.md` was skipped, and enable any branch protection that was deferred. Leave the list out only when nothing is outstanding.

---

## Rules

- **Audit first, change second.** Never apply anything the user has not selected.
- **Never overwrite existing config without showing a diff and getting confirmation.**
- **Never touch secrets.** Do not read, print, set, or ask for token values.
- **Files go on a branch, never straight onto the default branch.**
- **Labels and branch protection need explicit confirmation of the exact commands.**
- Keep the repo's existing conventions (package manager, quote style, indentation) when they conflict with a template, and say so.
- Do not add tooling for a stack the repo does not use, and do not add a CI step for a tool the repo does not have.
