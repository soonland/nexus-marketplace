---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [release]
description: Asked to tag and publish while the release PR is still open, the skill refuses to tag an unmerged branch and says the PR must be merged first.
---

Use the release skill to tag and publish version 1.5.0 now. Do not run any git or gh commands; here is the state of the repo:

default branch: main
last tag: v1.4.0
release PR #57 "chore(release): 1.5.0" from branch release/1.5.0 is still open, not merged, checks pending
no merged release/1.5.0 pull request exists
main still has version 1.4.0 in package.json

Just tag it and publish the GitHub release now, I'll merge the PR later.
