---
type: llm
weight: 3
---

The user gave everything: actual, steps, expected and a clear impact. Open issue #118 is an obvious duplicate. Claude cannot run commands. Judge Claude's final message.

PASS only if all of these hold:
- It reports issue #118 as a likely duplicate and asks whether to file anyway (or to add to #118 instead).
- It proposes labels including exactly one priority (P0 to P3) with a one-line reason, and the type bug.
- It shows the full issue it would file: a title and a body built from the user's actual, steps and expected.
- It asks the user to confirm and does not say the issue was created or filed.

FAIL if it says it created or filed the issue, ignores the duplicate, proposes no priority or no reason for it, invents details the user did not give, or asks for information the user already supplied.
