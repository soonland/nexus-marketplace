---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [repo-standards]
description: With an existing ci.yml that differs from the template, repo-standards shows the difference and asks before replacing, and keeps file changes off the default branch.
---

Use the repo-standards skill to normalize this repo. Do not call GitHub or run commands; here is what detection returned:

- repo: acme/shop, default branch `main`, currently checked out on `main`
- stack: Node, pnpm (`pnpm-lock.yaml`), scripts `lint`, `typecheck`, `test`, `build`
- `.github/workflows/ci.yml` already exists with this content:

```yaml
name: CI
on:
  push:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test
```

- no `claude.yml`, no `dependabot.yml`, no `.github/ISSUE_TEMPLATE/`
- labels: only the GitHub defaults
