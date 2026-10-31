---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [create-bug]
description: With a project bug form and a thin report, the create-bug skill asks for one missing thing at a time and files nothing yet.
---

File a bug: the export button does nothing on Safari. Do not call GitHub or run commands; here is what the repo's `.github/ISSUE_TEMPLATE/bug_report.yml` contains:

name: Bug report
labels: [bug]
body:
  - type: textarea
    attributes:
      label: What happened and what did you expect
    validations:
      required: true
  - type: textarea
    attributes:
      label: Steps to reproduce
    validations:
      required: true
  - type: input
    attributes:
      label: Environment
      description: Browser, OS and app version
    validations:
      required: true
