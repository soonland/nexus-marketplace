# `nexus-vitest` add-on: `vitest-unit-tester`

An optional agent for Vitest unit tests. Claude picks it on its own after new or changed functions, classes or modules that lack tests, or when you ask to raise coverage (for example "add tests for UserService" or "make sure the empty-array case is tested"). It writes new suites, edge-case and error-path tests, mocks, fakes and test utilities, and focuses on behavior rather than implementation details.

## Install
```
/plugin install nexus-vitest@nexus-marketplace
```

## What you'll see
1. **A project check first.** It reads `package.json` for the installed `vitest` version and related packages (coverage, Testing Library, jsdom or happy-dom, MSW, browser mode), then your Vitest config and setup files, then two or three existing test files. It follows your existing conventions over its own defaults.
2. **A case plan, then the tests.** Happy paths, edge cases, error conditions and boundary values, one behavior per test, with names that read as specifications.
3. **It runs the tests** with your project's package manager and fixes failures before reporting back. It tells you the command it ran and the result, and says so if it could not run them.

It writes for the installed version. Vitest 5 needs Node 22.12 or newer and Vite 6.4 or newer; if your project is on an older major, it writes for that version and briefly mentions what would change on upgrade. When unsure about an API, it checks the vitest.dev docs rather than guessing.

## What it changes
Test files and test utilities in your working tree. The agent keeps notes (Vitest version, config specifics, mocking patterns, how to run the suite) in its own user-level agent memory.

## What it never does
- End-to-end or browser tests of whole applications.
- Test private implementation details, unless you ask.
- Guess when the expected behavior is ambiguous: it asks. If the code is hard to test, it suggests a small refactor instead of writing contorted tests.

## Needs
A project with Vitest installed; the agent reads the version from your `package.json`.

## With the nexus skills
`implement-feature` delegates test writing to this agent when the add-on is installed. Without it, `implement-feature` writes the tests itself.
