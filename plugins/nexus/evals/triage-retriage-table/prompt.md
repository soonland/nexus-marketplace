---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
tags: [triage]
description: Re-triage of several issues produces one table of proposed label changes with a reason each, suggests feature-spec for an idea, closes nothing, and waits for acceptance.
---

Re-triage these three open issues. Do not call GitHub or run commands; here is what `gh issue view` returned:

#31 "Login fails for everyone"
labels: bug, P2
body: Filed last week as a minor annoyance. Update from the maintainer this morning: since the deploy at 08:00, every user gets a 500 on login in production. Nobody can sign in. No workaround.
comments: none other

#34 "Export to CSV drops the last row"
labels: bug
body: Steps: export a table with 3 rows from the Reports page. Actual: the CSV has 2 data rows. Expected: it has 3 data rows. Happens every time; a workaround is to add an empty row at the end.
comments: none

#36 "Would be great to have a dark mode"
labels: (none)
body: It would be nice if the app had a dark mode toggle in settings.
comments: none
