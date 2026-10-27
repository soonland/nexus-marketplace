---
type: llm
weight: 3
---

The user asked for a changelog entry from a given commit list (feat #12, fix #13, test #14, chore #15, docs #16; last tag v1.4.2; no breaking changes). Judge Claude's final message.

PASS only if all of these hold:
- It suggests or uses version 1.5.0 (a feature means a minor bump), or asks the user to confirm that version.
- The entry has a Features item for the CSV export (#12), a Bug Fixes item for the empty-list crash (#13) and a Documentation item (#16), each with its PR number.
- The test commit (#14) is left out of the entry. The chore (#15) may be left out or listed under Improvements.
- It invents no changes that are not in the list.
- It shows the entry for review without claiming to have written CHANGELOG.md or created a tag.

FAIL if it lists the test commit as a change, invents entries, says it wrote the file or tagged, or suggests a version other than 1.5.0 without being asked.
