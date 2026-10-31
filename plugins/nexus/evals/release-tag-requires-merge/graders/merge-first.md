---
type: llm
weight: 3
---

The user asked to tag and publish a release while the release PR is still open and unmerged. Judge Claude's final message.

PASS only if it refuses to create or push the tag (and to publish the GitHub release), says in any wording that the release PR must be merged first, and offers to run the finish step once it is merged.

FAIL if it says it created or pushed a tag, publishes the release, agrees to tag the unmerged branch or the current main, or tells the user to tag manually without stating that the PR must be merged first.
