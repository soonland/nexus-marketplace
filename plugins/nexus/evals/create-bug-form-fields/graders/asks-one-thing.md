---
type: llm
weight: 3
---

The user's report is thin: the export button does nothing on Safari. The project's bug form has three required fields (what happened and what was expected, steps to reproduce, environment). Claude cannot run commands. Judge Claude's final message.

PASS only if all of these hold:
- It asks for ONE missing thing, in plain words: for example the steps to reproduce, or what the user expected to happen, or the Safari version and OS. It may mention that it follows the form's fields.
- It does not list all the missing fields at once or present a long questionnaire.
- It does not ask for a commit SHA.
- It does not say or imply that an issue was created, and does not present a finished issue for filing yet.
- It does not guess the expected behavior or invent steps.

FAIL if it asks several questions at once, asks for a commit SHA, invents details, or says it filed or created the issue.
