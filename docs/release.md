# `/nexus:release [prepare | finish] [version]`

Releases through a release PR, in two phases. It never pushes to your default branch.

## Phase 1: prepare
1. Checks that the working tree is clean and the default branch is current.
2. Suggests a **version** from the commits since the last tag (breaking = major, or minor while `0.x`; `feat` = minor; `fix` = patch). You confirm.
3. Finds where your project keeps its version (`package.json`, `pyproject.toml`, `Cargo.toml`, `plugin.json`, Maven/Gradle, `VERSION`) or tells you it comes from tags alone. A lockfile that records the version is refreshed with the tool's own command.
4. Writes the **changelog entry** and shows it to you.
5. **Shows a summary and waits for your OK**, then pushes `release/<version>` and opens the PR `chore(release): <version>`.

## Phase 2: finish (after the PR is merged)
1. Checks that the release PR really is merged, and pulls the default branch.
2. **Asks, then tags** the merge commit `v<version>` and pushes the tag.
3. **Asks, then publishes** the GitHub release with notes from the changelog (marked pre-release for versions like `1.0.0-rc.1`).

With no argument it works out the phase from the repo, and stops if a release PR is still open.

## What it never does
- Push to the default branch, tag an unmerged branch, or get around a blocked push or tag (it reports the error instead).
- Publish to npm, PyPI or other registries; that stays with your CI or you.

## Needs
`git` and `gh`, authenticated.
