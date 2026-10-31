---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [fix]
description: Given an issue too vague to work on, the fix skill proposes one comment to the reporter asking for everything missing, and posts nothing without approval.
---

Fix issue #88. You cannot run commands here, so nothing can be posted or labelled. Do not call GitHub; here is what `gh issue view 88` returned:

author: some-customer
authorAssociation: NONE
labels: bug
title: Report export is wrong
body: the report export sometimes gives wrong totals

comments: none
