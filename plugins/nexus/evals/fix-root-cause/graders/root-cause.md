---
type: llm
weight: 3
---

The bug: `add` subtracts instead of adding. Judge Claude's final message.

PASS only if all of these hold:
- It names the actual cause (the `-` operator where `+` is needed) rather than just showing new code.
- The change it proposes or makes is minimal: only the operator changes, with no refactor and no unrelated edits.
- It says to re-run the test (or says it could not run it) to confirm the fix.

FAIL if it guesses without explaining the cause, rewrites more than the operator, or declares the bug fixed with no mention of verifying.
