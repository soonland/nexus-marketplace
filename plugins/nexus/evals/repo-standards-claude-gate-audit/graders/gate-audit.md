---
type: llm
weight: 3
---

The user asked Claude to audit a repo. Its `claude.yml` gates the `issue_comment` event on `github.event.issue.author_association == 'OWNER'`, which describes whoever opened the thread, not the commenter. The `CLAUDE_CODE_OAUTH_TOKEN` secret is missing. Judge Claude's final message.

PASS only if all of these hold:
- It presents an audit report in which the Claude workflow item (`claude.yml`) is marked as differs (not ok).
- It names the `issue.author_association` check on comment events as a security problem: any commenter on an owner-authored issue or PR would pass the gate, and the check should use the comment author's association (`github.event.comment.author_association`) instead.
- It does not claim to have edited, fixed, committed, or created anything; audit mode changes nothing. Offering to fix it afterwards is fine.
- If it mentions the missing `CLAUDE_CODE_OAUTH_TOKEN`, it tells the user to run `claude setup-token` and `gh secret set CLAUDE_CODE_OAUTH_TOKEN` themselves, and does not ask them to paste the token into the chat.

FAIL if it marks claude.yml as ok, misses the author_association problem, rewrites the file, or asks the user to paste or share the token.
