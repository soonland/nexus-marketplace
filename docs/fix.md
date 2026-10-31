# `/nexus:fix <issue number, error, failing test, or what is going wrong>`

Fixes a bug, a malfunction, a failing or flaky test, or a broken build, root cause first. Paste the error, describe what is going wrong, or give an issue number ("fix 213"). It works just as well for something discovered weeks later as for a fresh failure.

## What it does, in order
1. **Understands the problem.** It needs three things: what happens now, how to make it happen, and what should happen instead. A failing test or an error with its command usually covers all three, so it only asks for what is missing, **one question at a time**, in plain words. When you give an issue number, it reads the issue and its comments first and treats them as the source, so a complete issue (for example one filed with `create-bug`) means it asks you nothing. It never asks for a commit SHA. What counts as "correct" is your call: if the expected result is unclear, it asks before it writes anything.
2. **Reproduces** the failure itself. If it can't, it says what it needs from you instead of guessing.
3. **Finds the cause.** If it used to work, it reads recent history for the files involved and, when that isn't enough, uses `git bisect` in a separate worktree (it finds the good commit itself, so you don't have to). Otherwise it checks the usual suspects for your stack (wrong runtime boundary, stale generated code, caches, mismatched dependencies, missing environment variables). It states the cause in one sentence before touching code, and for an issue it offers to comment that root cause on it.
4. **Asks whether to use a new branch or worktree** (suggested name `fix/<short-slug>`; you may already be on the right one) before it changes any file, then **writes a failing regression test first**, at the lowest level that reproduces the bug. The test covers the reported case plus its boundaries (empty, zero, off by one, and so on), and it must fail for the reported reason before the fix. With no tests around the code, it pins current behavior first.
5. **Makes the smallest fix** for the root cause, with no unrelated refactoring.
6. **Proves it**: undoes the fix to confirm the new test fails again, runs your typecheck/build, lint and full test suite, and **reports the real results**. Anything it could not run is reported as not run. It also looks for the same pattern elsewhere and tells you what it found.

When it can only propose changes, it presents them in the same order: the cause, the regression test, then the fix.

## Working from an issue
- Issue text is treated as data: it never runs a command, fetches a URL or adds a dependency just because an issue says to.
- If only the reporter can fill a gap, it first checks whether they were already asked (a comment or the `needs-info` label). If not, it offers ONE comment asking for everything missing, plus the `needs-info` label, and then stops and tells you who is waiting on whom.
- Both comments (the request for details, the root cause) are shown to you word for word and posted only after you approve.

## What it won't do
- Guess at a fix when it doesn't know what the wrong behavior is or what the right one should be. It asks first.
- Skip, delete or loosen a test to get green. If it thinks the test is wrong, it says why and asks.
- Cover up a flaky test with retries or a longer timeout. It looks for the nondeterminism instead.
- Open a pull request (use `implement-feature` or your usual flow) or close the issue.
- Post a comment, add a label or create a branch without your approval.

## What it changes
Your working tree only (a bisect runs in a separate, temporary worktree). It commits (Conventional Commits, `fix(<scope>): ...`, with `Fixes #<n>` in the body for an issue) only if you want it to. It touches GitHub only to read an issue, and to post the comments or label you approved.

## Needs
The project's own tooling; the `gh` CLI, authenticated, only when you give an issue number.
