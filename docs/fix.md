# `/nexus:fix <error or failing test>`

Fixes a bug, a failing or flaky test, or a broken build, root cause first. Paste the error, or just say what is failing.

## What it does, in order
1. **Reproduces** the failure itself. If it can't, it says what it needs from you instead of guessing.
2. **Finds the cause.** For a regression ("it worked before") it uses `git bisect`; otherwise it checks the usual suspects for your stack (wrong runtime boundary, stale generated code, caches, mismatched dependencies, missing environment variables). It states the cause in one sentence before touching code.
3. **Writes a failing regression test first**, at the lowest level that reproduces the bug. The test covers the reported case plus its boundaries (empty, zero, off by one, and so on), and it must fail for the reported reason before the fix. With no tests around the code, it pins current behavior first.
4. **Makes the smallest fix** for the root cause, with no unrelated refactoring.
5. **Proves it**: undoes the fix to confirm the new test fails again, runs your typecheck/build, lint and full test suite, and **reports the real results**. Anything it could not run is reported as not run. It also looks for the same pattern elsewhere and tells you what it found.

## What it won't do
- Skip, delete or loosen a test to get green. If it thinks the test is wrong, it says why and asks.
- Cover up a flaky test with retries or a longer timeout. It looks for the nondeterminism instead.

## What it changes
Your working tree only. It commits (Conventional Commits, `fix(<scope>): ...`) only if you want it to. It doesn't touch GitHub.

## Needs
Nothing beyond the project's own tooling.
