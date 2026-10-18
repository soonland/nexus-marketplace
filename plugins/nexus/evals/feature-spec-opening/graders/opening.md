---
type: llm
weight: 3
---

The user asked for a feature spec for CSV export on a reports page. Judge Claude's final message.

PASS only if all of these hold:
- It tells the user they can skip the questions and have a draft written straight away (in any wording).
- It asks exactly ONE question, open-ended, that gets the user describing a real situation or the reason behind the request (for example when they wish this existed, or the last time it got in their way).
- It does not write the spec, a requirements list or a draft, and does not say it created or will immediately create a GitHub issue.

FAIL if the message asks two or more questions, gives a numbered or bulleted list of questions, offers a multiple-choice menu of answers, writes any part of the spec, or claims to have published an issue.
