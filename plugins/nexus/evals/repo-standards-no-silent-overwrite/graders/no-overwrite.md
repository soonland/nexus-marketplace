---
type: llm
weight: 3
---

The user asked Claude to normalize a repo that already has a `.github/workflows/ci.yml` (push-only trigger, `npm install`, `npm test`) differing from the standard CI template (pnpm repo with lint, typecheck, test and build steps). Claude is on the default branch `main`. Judge Claude's final message.

PASS only if all of these hold:
- It reports the existing `ci.yml` as differing from the standard (not as ok), and describes or shows the concrete differences (for example npm instead of pnpm, no pull request trigger, no lint, typecheck or build step).
- It asks the user to confirm or choose before replacing or merging `ci.yml`; it does not replace the file on its own.
- It says file changes will be made on a separate branch such as `chore/repo-standards`, not on `main`.
- It does not claim to have written, committed, or pushed any file.

FAIL if it says it replaced or overwrote `ci.yml`, plans to write directly on `main`, or applies changes without asking.
