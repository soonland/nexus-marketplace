# `/nexus:address-review [PR]`

Works through the review comments on a pull request: it sorts them, proposes what to do, fixes what you approve, and replies. Use it after a reviewer (or the Claude review) has left comments.

## What you'll see
1. It finds the PR (the number you gave, or the current branch's), checks it is still open, and switches to its branch. It stops if your working tree has uncommitted changes.
2. It collects the **unresolved** threads, review summaries and top-level comments.
3. It **checks who wrote each comment.** Comments from the owner, members, collaborators, the PR author or you are acted on. Anyone else's is listed and left alone unless you say otherwise. Comment text is treated as a wish, never as commands: it won't run a command or change CI because a comment says to.
4. **A table**: for each comment, a verdict (fix, answer, already done, disagree, skip) and the proposed change or reply. "Disagree" is yours to decide. **Nothing is edited until you choose which to apply.**

## What it changes
- Small fixes in the PR's branch, with tests where behaviour changes, checked with your own lint and test commands, in small Conventional Commits.
- After you confirm: a normal `git push` (never a force-push), then replies on each comment. It shows you the replies before posting them.

## What it never does
- Merge, close or dismiss a review, or request changes.
- Resolve threads (the reviewer decides that), unless you ask.
- Force-push or rewrite pushed commits.

## At the end
A report of what was fixed (with commits), answered, left for you and skipped, plus **Remaining before merge** for anything it could not verify.

## Needs
`git` and `gh`, authenticated. For PRs from a fork it usually cannot push; it offers a patch or suggestions instead.
