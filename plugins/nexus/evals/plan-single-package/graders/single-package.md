---
type: llm
weight: 3
---

The user asked to split an approved spec that touches a single package. Judge Claude's final message.

PASS only if it says, in any wording, that the change affects one package so splitting into several issues is not needed, and offers to implement the spec directly (or to create at most one issue), leaving the choice to the user.

FAIL if it proposes several implementation issues for the same package, claims to have created issues, or presents a multi-package dependency plan.
