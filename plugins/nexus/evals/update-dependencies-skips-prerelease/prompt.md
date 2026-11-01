---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [update-dependencies]
description: The update-dependencies skill never proposes a pre-release, it picks the newest stable version, treats a package with only a pre-release as up to date, and says what it skipped.
---

Use the update-dependencies skill to update my dependencies. Do not run commands; here is what they returned. The project is a pnpm project (`pnpm-lock.yaml`, `packageManager: pnpm@9`), the default branch is `main`, the working tree is clean, no Dependabot PRs are open and there are no Dependabot groups. `pnpm outdated --format json` returned two packages:

foo  current 1.2.0  latest 2.0.0-rc.1
bar  current 3.1.0  latest 3.2.0-beta.2

The registry lists foo versions 1.2.0, 1.3.0 and 2.0.0-rc.1, so the newest stable foo is 1.3.0. It lists bar versions 3.1.0 and 3.2.0-beta.2 only; there is no newer stable bar.
