# Security policy

## Supported versions

Only the latest release of each plugin is supported.

## Reporting a vulnerability

Do not open a public issue. Use the "Report a vulnerability" button under the repository's Security tab (private vulnerability reporting is enabled), or go directly to:

https://github.com/soonland/nexus-marketplace/security/advisories/new

Reports are handled on a best-effort basis by a single maintainer.

## In scope

Anything that could let a stranger trigger a workflow or run commands through this marketplace, for example:

- a skill or template, including the generated GitHub Actions workflows, that executes untrusted issue or PR text
- a workflow template with a wrong permission or trust check
- an MCP server or hook that runs with more access than it needs
- a leaked secret

## Out of scope

- Problems in Claude Code itself or in third-party packages. Report those upstream.
- Questions about usage. Use Discussions.
