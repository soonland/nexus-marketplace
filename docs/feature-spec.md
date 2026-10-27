# `/nexus:feature-spec`

Turns a rough idea into a written spec, published as a GitHub Issue. It writes a spec only; it never writes code.

## What you'll see
- A short conversation: **one question per message**. The first message also tells you that you can skip the questions and ask for a draft straight away.
- **Playbacks**: it describes back what it understood, marking what it guessed. Correct anything that is wrong.
- It may offer to look at your codebase first. You decide.
- A **size check**: small, project, or several independent projects. Very small changes (a label rename) get "this doesn't need a spec, shall I just do it?".
- The **full draft**, with your decisions (what you said) kept apart from its assumptions (what it guessed).

## What it changes
Nothing, until you approve the full spec. Then it:
- creates the issue `Feature Spec: <name>` with the `feature-spec` label and a component label (created if missing), assigned to the requester;
- adds it to your project board if one is configured or you give a URL.

## Next
- `/nexus:plan-issues <issue>` to split a multi-package spec, or `/nexus:implement-feature <issue>` to build it directly.

## Needs
`gh`, authenticated. The issue is created in the repo of the current folder.
