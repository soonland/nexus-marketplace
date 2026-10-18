---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [fix]
description: The fix skill explains the root cause and makes a minimal change.
---

Fix this: my test `expect(add(2, 3)).toBe(5)` fails with "received -1". The function is:

export function add(a: number, b: number): number {
  return a - b;
}
