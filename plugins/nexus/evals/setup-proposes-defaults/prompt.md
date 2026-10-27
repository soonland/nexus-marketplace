---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [setup]
description: setup proposes components and an assignee from the repo, states its defaults, and changes nothing before the user confirms.
---

Use the setup skill for this project. Do not call GitHub or run commands; here is what detection returned:

- repo: acme/shop (from `gh repo view`)
- logged-in user: jdoe
- top-level folders: apps/web, apps/api, packages/ui, docs
- no existing `.claude/nexus.json`
