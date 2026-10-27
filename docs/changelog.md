# `/nexus:changelog [version]`

Writes a changelog entry from merged PRs and Conventional Commits since the last tag.

## What you'll see
- Two questions: the **new version** (it suggests one: breaking change = major, `feat` = minor, `fix` = patch) and the **range** (default: last tag to `HEAD`).
- The generated entry, grouped as Features, Bug Fixes, Improvements and Documentation, with PR numbers. **You review it before it is written.**

## What it changes
Adds the entry to the top of `CHANGELOG.md` (creating the file if needed). Nothing else.

## Tagging
If your project releases through a release PR (protected default branch), it does not tag; use [`/nexus:release`](release.md) instead. Otherwise it offers to tag, and only does so on your confirmation.

## What it never does
Invent changes: everything comes from the git log and merged PRs.

## Needs
`git`; `gh` for PR titles and numbers (it falls back to git history without it).
