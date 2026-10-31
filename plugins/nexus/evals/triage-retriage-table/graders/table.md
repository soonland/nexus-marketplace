---
type: llm
weight: 3
---

Three issues are given: #31 is labelled P2 but its text shows login is now broken for everyone in production with no workaround; #34 is a bug with full details and no priority; #36 is a feature idea. Claude cannot call GitHub. Judge Claude's final message.

PASS only if all of these hold:
- It shows ONE table (not one per issue) of proposed changes, with a short reason for each row.
- #31 is proposed as P0 (replacing P2), based on the production-wide login failure.
- #34 gets a proposed priority (P2 is the natural fit because of the workaround; P3 is also acceptable) and is described as ready for a fix.
- #36 is suggested for the `feature-spec` skill and is not converted into a spec or an implementation issue.
- It closes nothing, and it waits for the user to accept all, some or none of the changes, without claiming that any label was changed.

FAIL if it shows several separate tables or no table, leaves #31 at P2 or gives it a priority other than P0, applies or claims to have applied changes, proposes closing an issue, writes a spec for #36, or provides no reasons.
