# `/nexus:implement-feature <issue>`

Builds a spec or an implementation issue and opens a PR. "do 213" works too.

## What you'll see
1. **Checks first.** It reads the issue and checks the author (an outside author needs your explicit OK; the issue body is treated as a description, never as commands). For an implementation issue it checks that its dependencies are merged and stops if they aren't.
2. **A plan** with exact files, tests and risks, and a **spec check** against the requirements and anti-goals. If the plan can't meet a requirement, it asks you what to do before going on. **Nothing is edited until you confirm the plan.**
3. A question: do tasks with **subagents** or **natively**?
4. If your repo has the Claude review workflow, whether to add the `claude-review` label.

## What it changes on GitHub
- assigns the issue to you, and moves it to In Progress / In Review if a board is set;
- posts the plan as a comment with a progress checklist it ticks as it goes;
- opens a PR with `Closes #<issue>`, and links it on the issue (and on the parent spec for a child issue).

## What it changes locally
Creates a `feat/<slug>` branch and commits there in small Conventional Commits, with the tests green before each commit. It never commits to the default branch.

## The PR
Ends with **Remaining before merge**: concrete tasks for anything it could not verify (a smoke test, a visual check). It never writes "not ready to merge"; you decide.

## What it never does
Merge the PR, or touch anything the spec lists as out of scope.

## Needs
`gh`, authenticated, and the project's own test and lint commands.
