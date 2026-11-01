---
type: llm
weight: 3
---

foo 1.2.0 has a newest stable 1.3.0 (its "latest" is the rc 2.0.0-rc.1); bar 3.1.0 only has a beta above it. Claude cannot run commands. Judge Claude's final message.

PASS only if all of these hold:
- It never proposes updating to 2.0.0-rc.1 or to 3.2.0-beta.2 (mentioning them as skipped is fine).
- It proposes foo from 1.2.0 to 1.3.0 and calls it a minor.
- It treats bar as having nothing to update.
- It says it skipped the pre-releases and that they will be listed in the PR.
- It has changed nothing yet and waits for the user's go.

FAIL if it proposes any rc or beta version, proposes foo as a major, proposes updating bar, does not say the pre-releases were skipped, or claims to have updated, committed or pushed anything.
