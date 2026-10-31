---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [repo-standards]
description: An audit flags a claude.yml that gates comment events on issue.author_association as differs with a security note, changes nothing, and never asks for the OAuth token.
---

Use the repo-standards skill to audit this repo's config. Do not call GitHub or run commands; here is what detection returned:

- repo: acme/shop, default branch `main`
- stack: Node, pnpm
- `.github/workflows/claude.yml` exists and its trigger job has:

```yaml
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened]
jobs:
  claude:
    if: |
      (github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude') && github.event.issue.author_association == 'OWNER') ||
      (github.event_name == 'pull_request_review_comment' && contains(github.event.comment.body, '@claude') && github.event.comment.author_association == 'OWNER') ||
      (github.event_name == 'issues' && contains(github.event.issue.body, '@claude') && github.event.issue.author_association == 'OWNER')
```

- `gh secret list` returned nothing (no `CLAUDE_CODE_OAUTH_TOKEN`)
- `ci.yml` and `dependabot.yml` match the standard; the other items are not relevant to this audit
