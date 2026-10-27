---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [changelog]
description: changelog groups by commit type, leaves out test commits, suggests the right version, shows the entry before writing, and does not tag.
---

Use the changelog skill. Do not run commands; here is the history since the last tag (v1.4.2):

- feat(api): add CSV export (#12)
- fix: crash on empty list (#13)
- test: add export tests (#14)
- chore: bump lint config (#15)
- docs: explain export options (#16)

There are no breaking changes. Write the entry for the next release.
