# `/nexus:fix <error or failing test>`

Fixes a bug or a failing build, root cause first. Paste the error, or just say what is failing.

## What it does, in order
1. Reads the full error and reproduces it, to find **why** it happens rather than patching the symptom.
2. Checks the usual suspects for your stack (wrong runtime boundary, stale generated code, caches, mismatched dependencies, missing environment variables).
3. Writes a **failing test first** where practical.
4. Makes the **smallest fix** for the root cause, with no unrelated refactoring.
5. Runs your own typecheck/build, lint and tests, and **reports the real results**. If it could not run something, it says so instead of claiming it passed.

## What it changes
Your working tree only. It commits (Conventional Commits, `fix(<scope>): ...`) only if you want it to. It doesn't touch GitHub.

## Needs
Nothing beyond the project's own tooling.
