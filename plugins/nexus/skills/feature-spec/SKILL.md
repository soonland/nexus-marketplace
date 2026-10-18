---
name: feature-spec
description: Discover what the user actually wants through a short conversation, then write it up as a decision-ready feature specification published as a GitHub Issue. Use when the user asks to "document a feature", "write a feature spec", "create a spec", "write up a feature", or wants to capture requirements for a new UI, API, or engine capability before implementing it.
---

# Feature Spec Skill

Find out what the user really wants, and why, before anything is built. Then capture it as a complete, decision-ready feature specification published as a GitHub Issue.

People often haven't finished working out what they want. Good questions help them think it through, and once you understand the *why*, you can make the many small decisions they never mention the way they would have. Your belief about what they want is usually thinner than it feels.

Requires the `gh` CLI, authenticated.

If `.claude/nexus.json` exists in the project (created by the `nexus:setup` skill), read it first and use it for defaults: `repo` as the target repository, `components` as the Scope options, `defaultAssignee` as the requester default, and `projectBoardUrl` for the project board step (skip those questions and just confirm). If it is missing, continue normally and mention that `nexus:setup` can store these defaults.

The issue is created in the developer's own project repository (the repo of the current working directory, or one they name), never in the repository this plugin is distributed from.

This skill writes a specification. It never writes product code, scaffolds, or installs anything. Reading and exploring the codebase is fine.

---

## When This Skill Applies

Activate when the user wants to:
- Document a new feature before implementation begins
- Capture requirements, decisions, and scope for a planned capability
- Produce a written spec to share with others or reference during implementation

---

## Step 1 — Triage: what do you already know?

Before replying, ask yourself: what do I know about what they want, and why?

- **The request already contains everything that matters** (a small, specific change with clear behaviour): do not run a long conversation. Draft the spec directly from the request, say which parts are your assumptions, and go to Step 5.
- **Something non-trivial is missing** (the usual case): start the conversation in Step 2.
- **The request is really several independent features:** say so, agree on an order, and spec them one at a time.
- **It is too small to deserve a spec** (a one-line fix, a copy change): say so and offer to just do it with `nexus:implement-feature` or `nexus:fix` instead of creating an issue. The user can override.

---

## Step 2 — The conversation

### The first message

Your first message is two things, in this order, and nothing else:

1. One line saying that if they would rather skip the questions and have you draft the spec straight away, they can say so. If they do, state your assumptions clearly, draft it, and go to Step 5.
2. One open question that gets them describing something concrete, for example "What's the moment you find yourself wishing this existed?" or "Walk me through the last time this got in your way."

### How to talk

- **One question per message**, ending the message, as a single sentence with no list of example answers hanging off it. Plain language, pitched just above the user's level. Match their vocabulary and avoid process jargon.
- **Open questions first.** Their own words carry intent that your options cannot. Offer a short menu (`AskUserQuestion`) only when they are stuck or when the choice is a fixed one such as the scope or component. When they say "just guess" or "what do you think?", propose something concrete.
- **Anchor in specifics:** "Walk me through the last time…", "If it could only do one of these well, which?", "What would make you wince if I got it wrong?" A guess they can correct ("I'm guessing this is because X keeps biting you?") often beats a question.
- **Be concise.** Lead with what matters and offer the rest.

### Ground to cover

This is territory, not a script. Never read it out as a list of questions, and skip what the user has already told you.

- What kind of thing this is, and what makes it special
- **Why now**, and what success looks like to them
- Goals, non-goals, and **anti-goals** (what would make this a failure even if it technically works)
- **Prior art:** what they have seen elsewhere and loved or hated
- **Current state:** what exists today and what they have already tried
- Who uses it and in what situation; what they see and do, from their perspective
- Must-haves versus nice-to-haves; non-functional needs (performance, accessibility, security, compatibility)
- **How it gets made, where it matters:** find out which technical choices the user wants to make, which they are handing to you, and what they already have to build on. Anything you choose goes into the spec as your call (an assumption), never as something they agreed to.
- The details they care about
- Decisions that have not been made yet, and whether each should be resolved now or deferred (for each one to resolve now, ask a targeted `AskUserQuestion` with clear options)

Identity details that can be inferred are not conversation topics. Use `AskUserQuestion` for them: the part of the system the feature belongs to (options inferred from the repo or `components`, plus "cross-cutting") and the requester (GitHub username, default `gh api user --jq '.login'`).

### Offer recon

Once you have a little grounding and looking would help, offer to go look: the codebase, existing screens or APIs, docs, or the web. Say what you would look for. The user decides whether you go. Use what you find to describe the current state accurately.

### Think wide privately

Before proposing anything, come up with several approaches and drop the weak ones, including any that fight what the user told you about their reasons. Show the comparison only when it helps them decide.

### Show, don't tell

For features people will look at (a screen, a layout, a flow, a sequence of steps), offer a quick text wireframe or a Mermaid diagram that they can react to, and include the one they settle on in the spec. Keep conceptual questions (scope, trade-offs) in plain text.

### Play it back as you go

As soon as you understand one part well enough to describe it (what it is for and who it serves, or how one piece behaves), describe that part back in about 200–300 words, then return to questions about the next part. Playback and questions alternate. **Mark anything you are guessing as your guess**; only what the user said goes in as theirs. End each chunk by asking what is wrong or missing.

---

## Step 3 — Size the work

Once you understand what they want, say the size in plain words (the user can override):
- **Small:** one package, a few requirements. A single spec, kept short.
- **Project:** several packages or capabilities. A full spec; suggest `nexus:plan-issues` afterwards to split it.
- **Several independent projects:** separate specs, one at a time.

If it grows mid-conversation, stop, say so, and step up a size.

---

## Step 4 — Draft and check it as a builder would

Draft the full spec using the Output Format below. Keep requirements implementation-neutral. Put your own guesses in **Assumptions**, not in Decisions.

**Builder check:** before showing the draft, review it as a talented builder who has never spoken to the user and must plan from this document alone. Use a fresh subagent if you can (give it only the draft and the question "what would you have to ask the requester before you could plan this?"); otherwise do it yourself. Sort what comes back into three piles:

- **Answered already** by the conversation or the project: put the answer in the spec.
- **Minor:** a sensible builder could settle it without changing what gets built. Settle it yourself and record it under Assumptions as your call.
- **Theirs:** only the user can answer, and the answer changes what gets built.

Bring the "theirs" pile to the user in **one message**, most important first, and update the spec with their answers. That is the only round of questions the check produces. Do not hand the user the draft until the check is done.

---

## Step 5 — Confirm and publish

**Hard gate:** nothing is published until the user has approved the full spec. "Sounds good" to an idea is not approval of a document they have not seen. Show the draft (or a faithful summary for a small spec), list the calls you made as assumptions so they can check them while reading, and ask for explicit approval.

Then:
- Resolve the target repository — always the developer's own project repo, never this plugin's or marketplace's repo:
  ```bash
  REPO=$(gh repo view --json nameWithOwner --jq '.nameWithOwner')
  ```
  Run it from the user's working directory. If that fails (not a git repo, or no GitHub remote), ask the user for `owner/repo`. Otherwise confirm `$REPO` with the user (default to it) and let them override it.
- Use `--repo "$REPO"` on every `gh issue` and `gh label` command below, so the issue lands in that repo regardless of the working directory.
- Check for existing open issues labelled `feature-spec` in that repo (`gh issue list --repo "$REPO" --label feature-spec`) and flag any overlap
- Ask for confirmation before creating the issue, naming `$REPO` in the prompt
- Create the `feature-spec` label (color `#0075ca`) and a component label for the Scope answer (color `#ededed`) in `$REPO` if they don't already exist (`gh label create --repo "$REPO" ...`)
- Create the GitHub Issue using `gh issue create --repo "$REPO"` with:
  - Title: `Feature Spec: <Feature Name>`. Pass it through a variable assigned from a quoted heredoc (`TITLE=$(cat <<'EOF'` ... `EOF` ... `)`), then `--title "$TITLE"`. Never type it inside double quotes, so `$(`, backticks or `"` in the name cannot be run by the shell.
  - Labels: `feature-spec` + the component label
  - Assignee: the requester's GitHub username (`--assignee <username>`)
  - Body: the spec content formatted as markdown (pass via HEREDOC)
- If `projectBoardUrl` is not in the config, ask the user: "What is the GitHub project board URL for this issue? (e.g. https://github.com/users/OWNER/projects/N or https://github.com/orgs/ORG/projects/N — leave blank to skip)"
- If a URL is available, add the issue to that project as `Todo`:
  ```bash
  # Parse owner name and project number from the URL
  # e.g. https://github.com/users/octocat/projects/5 → owner=octocat, number=5
  PROJECT_OWNER=<owner from URL>
  PROJECT_NUMBER=<number from URL>

  # Resolve project node ID and Status field metadata dynamically
  PROJECT_ID=$(gh project view "$PROJECT_NUMBER" --owner "$PROJECT_OWNER" --format json | jq -r '.id')
  STATUS_FIELD_ID=$(gh project field-list "$PROJECT_NUMBER" --owner "$PROJECT_OWNER" --format json \
    | jq -r '.fields[] | select(.name == "Status") | .id')
  TODO_OPTION_ID=$(gh project field-list "$PROJECT_NUMBER" --owner "$PROJECT_OWNER" --format json \
    | jq -r '.fields[] | select(.name == "Status") | .options[] | select(.name | test("(?i)todo|backlog|ready")) | .id' \
    | head -1)

  ITEM_ID=$(gh project item-add "$PROJECT_NUMBER" --owner "$PROJECT_OWNER" --url <issue-url> --format json | jq -r '.id')

  gh project item-edit \
    --project-id "$PROJECT_ID" \
    --id "$ITEM_ID" \
    --field-id "$STATUS_FIELD_ID" \
    --single-select-option-id "$TODO_OPTION_ID"
  ```
- Return the issue URL to the user and mention the next steps: `nexus:plan-issues` to split it into one issue per package, or `nexus:implement-feature` to implement it directly

---

## Output Format

Use this structure for the GitHub Issue body. Omit optional sections that have nothing to say.

```markdown
# Feature Spec: <Feature Name>

**Status:** Draft
**Date:** <today's date>
**Scope:** <component name | cross-cutting>

---

## Problem Statement

<Why this feature is needed and why now. 2–4 sentences.>

---

## Current State & Prior Art

<Optional. What exists today, what has been tried, and what the user has seen elsewhere that they loved or hated.>

---

## Goals

- <Goal 1>
- <Goal 2>
- <Goal 3>

---

## Non-Goals

- <Explicit exclusion 1>
- <Explicit exclusion 2>

**Anti-goals** (would make this a failure even if it technically works):
- <Anti-goal>

---

## Users

<Who will use this feature and in what context.>

---

## Requirements

### <Capability Group 1>

- <Requirement> *(must have | nice to have)*
- <Requirement>

### <Capability Group 2>

- <Requirement>

### Non-Functional Requirements

- <Performance / accessibility / security / compatibility requirement>

### Constraints

<Optional. Technical or practical choices the user made or the project imposes (existing stack, services, deadlines). Only what they decided.>

---

## Decisions

| Question | Decision |
|---|---|
| <Question> | <Answer the user gave> |

(Omit this section if there were no open questions to resolve. Only the user's own decisions go here.)

---

## Assumptions

<Optional. Calls the author made that the user did not state, each marked as an assumption so they can be checked. Anything here is open to change; it is not settled.>

- <Assumption and why>

---

## Success Metrics

- <How you know this shipped correctly and is working>

---

## Out of Scope

- <Item>
- <Item>
```

---

## Rules

- **Never create the issue before the user approves the full spec.**
- **Decisions are the user's; assumptions are yours.** Never present a guess or a default you chose as something the user agreed to. Say your defaults out loud so they can reject them.
- Always resolve open questions before publishing — a spec with unresolved questions that change what gets built is not ready. Minor ones the builder could settle go under Assumptions.
- Keep requirements descriptive but implementation-neutral — don't prescribe how to build it, only what it must do. Technical choices appear only as user-stated Constraints.
- Do not build anything. This skill ends at the published issue.
- Use a HEREDOC when passing the body to `gh issue create` to preserve markdown formatting.
- After publishing, return the issue URL.

## Common traps

| Thought | Reality |
|---|---|
| "I'll offer options to save them effort" | Options steer. Ask them to describe it first. |
| "I'll fill in sensible defaults" | Fine, but say them out loud and record them as assumptions. A default they never heard is one they could not reject. |
| "I should explain the process first" | Ask your question. The process shows itself. |
| "They said 'sounds good' to the idea" | Approval covers what you showed them. A spec you haven't shown is not approved. |
| "I'll ask everything now" | One question per message; the rest can wait or may become unnecessary. |
