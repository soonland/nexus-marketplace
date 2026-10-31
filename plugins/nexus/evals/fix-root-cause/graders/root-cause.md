---
type: llm
weight: 3
---

The bug: `add` subtracts instead of adding. Judge Claude's final message.

PASS only if all of these hold:
- It names the actual cause (the `-` operator where `+` is needed) rather than just showing new code.
- The change it proposes or makes to the code is minimal: only the operator changes, with no refactor and no unrelated edits. A regression test for the bug is expected and does not count against this.
- It reports verification honestly: either the actual result of running the tests, or an explicit statement that it did not run them. It must not claim the bug is verified fixed without having run anything.

FAIL if it guesses without explaining the cause, changes more of the function than the operator, or claims the fix is verified when it ran nothing.
