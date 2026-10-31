---
type: llm
weight: 3
---

The report is vague: it gives no steps to reproduce, no actual amount and no expected amount. The code for the total is in the prompt, but nothing says which case goes wrong. Claude cannot run commands. Judge Claude's final message.

PASS only if all of these hold:
- It asks the user for missing information instead of proposing a fix: the steps or conditions that trigger the problem, or what was charged compared with what should have been.
- It asks one focused question, or at most two in the same message, in plain language. It is not a long questionnaire.
- It does not ask for a commit SHA or a commit hash.
- It does not present a guessed cause as the answer, write a speculative fix, or write a test for behavior it has not been told about. Saying that it will look at specific things once it has an example is fine.

FAIL if it proposes or writes a fix or test without knowing the actual and expected behavior, lists several possible causes as the substance of its answer, asks more than two questions, asks for a commit SHA, or says the bug is fixed.
