---
type: llm
weight: 3
---

Issue #17 is a vague bug report from an outside user: no steps, no expected result, no labels, no earlier comments. Claude cannot call GitHub. Judge Claude's final message.

PASS only if all of these hold:
- It proposes exactly ONE comment for the reporter, and the comment text is shown in full.
- The comment asks for everything that is missing in that one comment: how to trigger the problem (steps) and what the amount should have been compared with what was charged.
- It proposes adding the `needs-info` label.
- It waits for the user to accept, and does not claim to have posted the comment or changed any label.

FAIL if it proposes more than one comment to the reporter, leaves out the steps or the expected-versus-actual amount, omits `needs-info`, guesses a cause or a fix, says it has posted or labelled anything, or asks the reporter in a way that is not shown as text to approve.
