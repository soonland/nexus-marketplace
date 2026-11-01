---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [fix]
description: Given a complete issue, the fix skill takes actual, steps and expected from it, asks nothing about them, and shows the cause and regression test before the fix.
---

Fix issue #213. You cannot run commands here, so nothing can be posted or created. Do not call GitHub; here is what `gh issue view 213` returned:

author: maria-dev
authorAssociation: OWNER
labels: bug
title: pageCount returns one page too many when the total is an exact multiple of the page size
body:
**Actual:** `pageCount(20, 10)` returns 3. The list shows an empty third page.

**Steps:**
1. Call `pageCount(20, 10)`.
2. Compare with `pageCount(21, 10)`, which correctly gives 3.

**Expected:** 20 items on pages of 10 give 2 pages.

The function:

export function pageCount(total: number, pageSize: number): number {
  return Math.floor(total / pageSize) + 1;
}

comments: none
