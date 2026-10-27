---
name: fix
description: Fix a bug or failing build with a root-cause-first routine. Use when the user reports an error, a failing test, a broken build, or says "fix this".
argument-hint: "[error message or failing test]"
---

# Bug Fix Skill

1. **Read the error and find the root cause.** Read the full message and stack trace, reproduce the failure, and identify why it happens. Do not patch symptoms.
2. **Check the usual suspects for the project's stack.** Inspect the manifests, config files and `CLAUDE.md` to see what applies, for example:
   - Framework boundaries: code running on the wrong side of a runtime or serialization boundary (for example server versus client rendering, or values that cannot cross a process or API boundary).
   - Generated code: a schema-driven client (Prisma, GraphQL codegen, OpenAPI) that needs regeneration after the schema changed.
   - Stale state: build caches, lockfile or dependency mismatches, or environment variables that are missing or out of date.
3. **Write a failing test first** where practical, so the fix has a regression test.
4. **Implement the smallest fix** that addresses the root cause. Do not refactor unrelated code.
5. **Verify** with the project's own commands: typecheck or build (if the project has one), lint, and the relevant tests, then the full test suite. Report the actual results.
6. **Commit** using Conventional Commits (`fix(<scope>): <description>`) when the user wants it committed, ending the message with whatever attribution lines your environment instructs, if any.
