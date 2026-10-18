---
name: changelog
description: Generate or update a CHANGELOG.md from merged PRs and git history. Use this skill when the user asks to "generate a changelog", "update the changelog", "what shipped in this release", or wants to summarise changes for a version.
---

# Changelog Skill

Generate or update `CHANGELOG.md` from merged PRs and Conventional Commits since the last release.

Requires `git`, and the `gh` CLI (authenticated) if you want PR titles and numbers. Works in the developer's own project repo (the current working directory).

---

## When This Skill Applies

- Before tagging a new release
- After a batch of PRs have been merged
- When the user wants a human-readable summary of what changed

---

## Protocol

### Step 1 — Determine Range

Resolve the repo and default branch first:

```bash
REPO=$(gh repo view --json nameWithOwner --jq '.nameWithOwner')   # or repo from .claude/nexus.json
DEFAULT=$(gh repo view --json defaultBranchRef --jq '.defaultBranchRef.name')
```

If `gh` is unavailable or the repo has no GitHub remote, skip the PR lookup and use git history only.

Ask the user:
1. What is the new version number? (for example `1.2.0`; follow semver: `feat` = minor bump, `fix` = patch bump, breaking change = major bump). Suggest a version based on the commits.
2. What is the range? Default: from the last git tag to `HEAD`.

```bash
# Find the last tag
git describe --tags --abbrev=0 2>/dev/null || echo "no previous tag"

# List commits since last tag
git log <last-tag>..HEAD --oneline
```

If there is no previous tag, generate from the beginning of the repo.

### Step 2 — Collect Changes

Gather merged PRs and commits in the range:

```bash
# Conventional commits since last tag
git log <last-tag>..HEAD --pretty=format:"%s|%H|%an" --no-merges
```

Also fetch merged PR titles for richer descriptions:
```bash
gh pr list \
  --repo "$REPO" \
  --state merged \
  --base "$DEFAULT" \
  --json number,title,mergedAt,labels \
  --jq 'sort_by(.mergedAt)'
```
Only keep PRs merged after the date of the last tag.

### Step 3 — Generate Entry

Produce a changelog entry grouped by Conventional Commit type. Map types to human-friendly headings:

| Commit type | Changelog heading |
|---|---|
| `feat` | ### Features |
| `fix` | ### Bug Fixes |
| `chore` / `refactor` | ### Improvements |
| `test` | *(omit from changelog)* |
| `docs` | ### Documentation |

Format:

```markdown
## [<version>] — <YYYY-MM-DD>

### Features
- **<scope>**: <description> (#<PR number>)

### Bug Fixes
- <description> (#<PR number>)

### Improvements
- <description> (#<PR number>)
```

Link each entry to its PR number where available. Omit sections that have no entries.

### Step 4 — Write

If `CHANGELOG.md` exists, prepend the new entry below the top-level heading. If it doesn't exist, create it with a standard header:

```markdown
# Changelog

All notable changes to this project will be documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [<version>] — <YYYY-MM-DD>
...
```

### Step 5 — Tag

After writing, offer to create a git tag:
```bash
git tag -a v<version> -m "Release v<version>"
git push origin v<version>
```
Do not tag or push without the user's confirmation.

---

## Rules

- Use PR titles over raw commit messages when available; they are more readable.
- Skip `test:` and `chore:` commits unless they represent significant dependency upgrades.
- Never invent changes. Only include what is in the git log or merged PRs.
- If a commit doesn't follow Conventional Commits format, make a best-effort categorisation based on the message content.
- Always show the generated entry to the user for review before writing to the file.
