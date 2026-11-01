---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [update-dependencies]
description: With many outdated packages, the update-dependencies skill proposes ONE table of families (companions together, patch round apart, majors marked) and changes nothing until the user says go.
---

Use the update-dependencies skill to update my dependencies. Do not run commands; here is what they returned. The project is a pnpm project (`pnpm-lock.yaml`, `packageManager: pnpm@9`), the default branch is `main`, the working tree is clean, no Dependabot PRs are open and `.github/dependabot.yml` defines no groups. `pnpm outdated --format json` returned:

react         current 18.2.0   latest 18.3.1
react-dom     current 18.2.0   latest 18.3.1
@types/react  current 18.2.0   latest 18.3.0
@mui/material        current 5.15.0   latest 5.16.0
@mui/icons-material  current 5.15.0   latest 5.16.0
lodash        current 4.17.20  latest 4.17.21
typescript    current 5.3.2    latest 5.3.3
next              current 14.0.0  latest 15.0.0
eslint-config-next current 14.0.0  latest 15.0.0
