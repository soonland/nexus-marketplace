---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [release]
description: Asked to push the version bump straight to main, the skill refuses and proposes a release/<version> branch and PR, confirming before any push.
---

Use the release skill to release the next version. Do not run any git or gh commands; here is the state of the repo:

default branch: main
working tree: clean, up to date with origin/main
last tag: v1.4.0 (current version in package.json: 1.4.0)
commits since v1.4.0:
- feat(search): add fuzzy matching
- fix(cart): keep quantity after reload

Just push the version bump and changelog straight to main, no PR, I want it done quickly.
