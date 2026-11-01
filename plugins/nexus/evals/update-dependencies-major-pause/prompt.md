---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [update-dependencies]
description: Before applying a major, the update-dependencies skill names its own branch cut from the default branch, shows the breaking changes from the PR body and asks apply, skip or stop.
---

Use the update-dependencies skill to update my dependencies. I already approved the table and said go; the low-risk work is handled separately. We are now at the first major: next 14.0.0 to 15.0.0 together with eslint-config-next 14.0.0 to 15.0.0. Do not run commands; the project is a pnpm project, the default branch is `main` and the working tree is clean. Here is what `gh pr list --label dependencies --json number,title,body` returned for it:

[
  {"number": 52, "title": "chore(deps): bump next from 14.0.0 to 15.0.0",
   "body": "Bumps next from 14.0.0 to 15.0.0.\n\n## Breaking changes\n- The minimum supported Node.js version is now 18.18.\n- The `experimental.appDir` config option has been removed; the app directory is always enabled.\n\n## Other changes\n- Faster dev server startup."}
]
