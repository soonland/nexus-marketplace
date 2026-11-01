# Ecosystem reference

Per-stack commands for the update-dependencies skill. Detection matches the "Stack detection" section of the repo-standards skill. In a repo with several stacks, handle each one with its own row. In a workspace or monorepo, use the root's manager.

"Wanted" is the highest version inside the declared range. "Latest" is the newest published version. Commands marked "(check --help before use)" could not be verified in the environment where this file was written: read the tool's `--help` first and adjust.

## Detection

| Manager | Detected by |
|---|---|
| npm | `package-lock.json`, or `packageManager` starts with `npm@` |
| pnpm | `pnpm-lock.yaml`, or `packageManager` starts with `pnpm@` |
| yarn classic | `yarn.lock` without `.yarnrc.yml` |
| yarn berry | `yarn.lock` with `.yarnrc.yml` |
| bun | `bun.lock` or `bun.lockb` |
| uv | `uv.lock` (with `pyproject.toml`) |
| poetry | `poetry.lock`, or `[tool.poetry]` in `pyproject.toml` |
| pip | `requirements*.txt` and none of the above |
| Go | `go.mod` |
| Cargo | `Cargo.toml` (`Cargo.lock` holds resolved versions) |
| Maven | `pom.xml` |
| Gradle | `build.gradle` or `build.gradle.kts` |

For Node, the `packageManager` field in `package.json` wins over the lockfile. If nothing identifies the manager, ask.

## List outdated and update one package

Always update one package to an explicit version. Never run a blanket update-all.

| Manager | List outdated (current, wanted, latest) | Update one package |
|---|---|---|
| npm | `npm outdated --json` (add `--all` for transitive) | `npm install <pkg>@<version>` and add `--save-dev` for a dev dependency (`--save-exact` if the manifest pins exact versions) |
| pnpm | `pnpm outdated --format json` (`--long` for details; `--dev` or `--prod` to filter; `--recursive` in a workspace) | `pnpm add <pkg>@<version>` and add `--save-dev` for a dev dependency (`--save-exact` if pinned); in a workspace add `--filter <workspace>` |
| yarn classic | `yarn outdated --json` (check --help before use) | `yarn upgrade <pkg>@<version>` (check --help before use); keeps the group |
| yarn berry | No built-in non-interactive list (`yarn upgrade-interactive` is interactive, do not use it): report as unavailable (from memory; check --help before use) | `yarn up <pkg>@<version>` (check --help before use) |
| bun | `bun outdated` (check --help before use) | `bun add <pkg>@<version>` and add `--dev` for a dev dependency (check --help before use) |
| pip | `pip list --outdated --format=json` (check --help before use; lists installed packages, not the requirements files) | Edit the pin in the requirements file, then `pip install -r <file>` (check --help before use) |
| uv | `uv pip list --outdated` or `uv tree --outdated` (check --help before use) | `uv lock --upgrade-package <pkg>==<version>` then `uv sync`; to change the declared range use `uv add <pkg>==<version>` (add `--dev` for dev group) (check --help before use) |
| poetry | `poetry show --outdated` (check --help before use) | `poetry add <pkg>@<version>` (add `--group dev` for a dev group) (check --help before use) |
| Go | `go list -m -u -json all` (check --help before use) | `go get <module>@v<version>` then `go mod tidy` (check --help before use) |
| Cargo | `cargo outdated` (separate plugin, see below) | `cargo update -p <crate> --precise <version>` for the lockfile; to change the declared requirement use `cargo add <crate>@<version>` (add `--dev` or `--build` to keep the table) |
| Maven | `mvn versions:display-dependency-updates` (needs the versions plugin, see below; check --help before use) | Edit the version in `pom.xml` (or the property that holds it), then resolve with the project's usual build command |
| Gradle | Needs a plugin, see below | Edit the version in the build file or version catalog (`gradle/libs.versions.toml`), then refresh with the project's usual build command |

Notes:
- Lockfile refresh: npm, pnpm, bun, uv, poetry, Go (`go.sum`) and Cargo rewrite the lockfile as part of the command. pip has no lockfile: the pinned requirements file is the record. Maven has none by default.
- pnpm and npm output: the `wanted` field is in-range, `latest` is newest. For tools that print only current and latest, treat wanted as unknown and say so.
- Pip: if a project uses `pip-tools`, the `.in` file is the source and the `.txt` is compiled. Edit the `.in` and recompile with the project's own command.

## Tools that need an extra install

| Stack | Needs | If missing |
|---|---|---|
| Cargo | `cargo outdated`, a separate cargo plugin (`cargo install cargo-outdated`; check --help before use) | Report listing as unavailable. Do not install it unless the user asks. |
| Maven | the `versions-maven-plugin` (invoked as `versions:`; works without declaring it, but needs network) | Report as unavailable if the plugin cannot be resolved. |
| Gradle | a plugin such as `com.github.ben-manes.versions` (name from memory; check before use), or a manual check of the registry | Report as unavailable. Do not add a plugin to the project's build file without the user's approval. |
| yarn berry | no built-in non-interactive outdated list | Report as unavailable. |

When a listing tool is unavailable, the skill says so plainly for that stack. It does not invent a replacement, guess versions, or scrape the registry by hand.

## Pre-release detection

Never take a pre-release version, even when it is the "latest" tag.

| Scheme | Pre-release markers |
|---|---|
| npm, pnpm, yarn, bun, Cargo (semver) | hyphen suffix: `-rc.1`, `-beta.2`, `-alpha`, `-next`, `-canary`, `-dev` |
| Python (PEP 440) | `a1`, `b2`, `rc1`, `.dev3` |
| Go | `-rc`, `-beta`, `-alpha`, `-pre` suffix in the module version |
| Maven, Gradle | `-SNAPSHOT`, `-RC1`, `-M1`, `-alpha`, `-beta`, `-CR1` |

If the newest version is a pre-release, take the newest stable one instead. If none is newer than the current one, the package is not outdated.

## Classifying a version change

Compare old and new versions as semver `MAJOR.MINOR.PATCH`:
- MAJOR differs: major.
- MAJOR same, MINOR differs: minor.
- Only PATCH differs: patch.

For `0.x` versions, treat a minor bump (`0.4.2` to `0.5.0`) as potentially breaking, so handle it like a major. For non-semver schemes (calendar versions, four-part Maven versions, Python versions without semver discipline), the classification is a best guess: say so in the report.

## Declared companions (peer dependencies)

Only npm-family manifests (`package.json` for npm, pnpm, yarn, bun) have `peerDependencies`. The peer-dependency grouping rule applies there only. For every other stack, skip it.

## Release notes

For each bump, in order:
1. The Dependabot PR body, when there is one.
2. The package's own repository releases: `gh release list --repo <owner/repo>` then `gh release view <tag> --repo <owner/repo>`. Find the repository from registry metadata (npm: `npm view <pkg> repository.url`; for other stacks use the repository link in the package's registry page or manifest metadata).
3. If no source is readable, say so. Do not summarize from memory.

## Safety notes

- Installing a new version may run lifecycle or install scripts (npm `postinstall`, build scripts, native builds). Mention this in the hand-over.
- Do not add flags that change the project's normal install behaviour (for example `--ignore-scripts`, `--force`, `--legacy-peer-deps`) unless the user asks.
