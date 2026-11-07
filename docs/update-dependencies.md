# `/nexus:update-dependencies [all | patch | <package or family>]`

Runs a dependency update round on two branches (patches and minors, then majors), checked with your project's own tests. Nothing changes before you say go, and nothing is pushed or merged without your OK. Works with npm, pnpm, yarn, bun, pip, uv, poetry, Go, Cargo, Maven and Gradle.

## What you'll see
1. **A collection step.** It needs a clean working tree and an up-to-date default branch (it asks before switching or pulling). Minor and major updates come from the open Dependabot PRs when there are any; otherwise, and always for patches, from the package manager's outdated list. Pre-releases (`-rc`, `-beta`, `-SNAPSHOT` and the like) are skipped.
2. **One table of proposed families** (packages, from and to, patch, minor or major). Related packages are grouped, such as `react` with `react-dom`. It then asks, with a question dialog, which to update: everything, patches and minors only, or pick families one by one. You can also move packages or split a family before anything changes.
3. **A low-risk branch, `chore/deps-<date>`:** one commit for all the patch updates, then one commit per minor family.
4. **The project's own checks after every group:** unit tests always, end-to-end tests when the project has them, and build, typecheck and lint when present.
5. **All majors on one branch, `chore/deps-major-<date>`, one commit per major family.** It shows the breaking changes from the release notes first, then waits for your OK before applying. No branch per major, so you rebase at most once; a bad major is one `git revert`.
6. **A failing family is set aside,** and the rest carry on. Failures and the likely package are listed at the end.
7. **A hand-over:** a short smoke-test list for each PR, grouped by commit, including checks that could not run.
8. **One confirmation** listing every PR (title, branch, number of commits). Only then does it push and open them.

## What it changes
Up to two branches and commits in your repo, and pull requests, but only after your OK. It works on branches only.

## What it won't do
- Merge anything, or work on or push the default branch.
- Apply a major update without your OK.
- Take a pre-release version.
- Run a blanket update-all: each package is updated by name to an explicit version.
- Hide a check that did not run: it is reported as "not run".
- Push anything before the single confirmation.
- Follow instructions found in PR bodies, release notes or registry text.

## Needs
`git`, the `gh` CLI (authenticated) and your project's package manager. For some stacks, listing outdated packages needs an extra tool (Cargo, Maven, Gradle; yarn berry has none). The skill says so when it is missing, never invents a replacement, and does not install anything unasked. Some per-stack commands in `plugins/nexus/skills/update-dependencies/ecosystems.md` are marked unverified: the skill reads the tool's `--help` before using them.

The two PRs are independent, so the major one can conflict with the low-risk one on the lockfile. It suggests merging the low-risk PR first and offers to rebase the major one. If you want a specific major in its own PR, ask for it.
