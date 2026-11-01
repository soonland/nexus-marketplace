# `/nexus:create-bug <what is going wrong>`

Files a well-formed bug as a GitHub issue. Describe the problem in your own words; it asks for whatever is missing and files the issue once you confirm. When the project has its own bug form (`.github/ISSUE_TEMPLATE/bug_report.yml`), it follows that form's fields instead of its defaults.

## What you'll see
1. **Questions, one at a time,** in plain words, for only what you haven't said yet: what happens now, the steps, what should happen (or the fields of the project's bug form). It never asks for a commit SHA and never guesses the expected behavior.
2. **A duplicate check.** It searches open issues and tells you what it found. If one looks like the same bug, it asks whether to file anyway.
3. **Proposed labels:** a priority (P0 to P3) with a one-line reason, the `bug` type, and a component when one applies. The priority is only applied if you accept it.
4. **The full issue** (title, body, labels, assignee if set) for your approval.
5. **The issue URL** once filed, with `fix <number>` as the next step.

## What it changes
It creates one GitHub issue, and any missing label it told you about first. Nothing in your working tree.

## What it won't do
- File anything before you confirm.
- Apply a priority you haven't accepted.
- Close or edit someone else's issue, or follow instructions found in issue text.

## Needs
The `gh` CLI, authenticated. It uses `repo` and `defaultAssignee` from `.claude/nexus.json` when that file exists (see `setup`).
