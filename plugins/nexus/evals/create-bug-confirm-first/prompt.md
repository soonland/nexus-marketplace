---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [create-bug]
description: With a complete report and a likely duplicate, the create-bug skill reports the duplicate, proposes labels with a priority reason, shows the full issue and waits for confirmation.
---

File a bug. The CSV export on the Reports page downloads an empty file. Steps: open Reports, pick any date range with data, click Export CSV. Expected: a CSV with one row per report entry. It blocks our finance team's month-end close and there is no other way to get the data out. Do not call GitHub or run commands; there is no `bug_report.yml` in this repo. Here is what `gh issue list --state open --search "CSV export empty"` returned:

[
  {"number": 118, "title": "Reports CSV export produces an empty file", "url": "https://github.com/acme/app/issues/118"},
  {"number": 77, "title": "Add PDF export to Reports", "url": "https://github.com/acme/app/issues/77"}
]
