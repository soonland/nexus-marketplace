---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [plan-issues]
description: A spec that touches one package is not split; the skill offers to implement it directly and creates nothing.
---

Use the plan-issues skill on this spec, which is already approved. Do not look anything up on GitHub; the full text is here.

Feature Spec: Show build date in footer
Status: Approved
Scope: web
Requirements:
- The footer of the web app shows the build date, formatted YYYY-MM-DD.
Out of Scope: any other page, any change to the API.

The repo is a single-package project (one `package.json`, no workspaces).
