# `nexus-mui` add-on: `mui-layout-designer`

An optional agent for Material UI (MUI) layouts. Claude picks it on its own when you create or restructure frontend components where layout or visual design matters, for example a dashboard with a sidebar and navbar, a responsive card grid, or a messy form to clean up. It covers page structure, responsive grids, forms, navigation and theming.

## Install
```
/plugin install nexus-mui@nexus-marketplace
```

## What you'll see
1. **A version check first.** It reads your `package.json` for `@mui/material`, `@mui/icons-material`, any `@mui/x-*` packages, `@mui/material-nextjs` and React, and notes your framework. It writes code for the MUI version you have installed, not the one it remembers. If none is installed, it uses the latest stable (v9 at the time of writing).
2. **Docs lookups.** It uses the bundled MUI MCP server (`@mui/mcp`) to fetch the docs for your installed major version before writing code for anything it is not sure about.
3. **Complete components.** The structure comes first (outer container, regions, then the grid or stack inside each), then runnable React and TypeScript code that uses theme tokens instead of hardcoded values.
4. **A check before it hands over:** responsive at every breakpoint, no removed or deprecated APIs for your version, accessible names and keyboard paths.

If you are on an older major (v5, v6 or v7), it writes for that version and briefly points to the v9 upgrade guide. It does not mix APIs from different majors.

## What it changes
Only the frontend files you asked it to build or restructure. The agent keeps notes (project MUI version, theme decisions, layout conventions) in its own user-level agent memory.

## What it never does
- Backend logic.
- Non-MUI styling systems.
- Invent doc URLs: it only fetches URLs returned by the MUI docs index. If the MCP tools are unavailable, it says so and falls back to the versioned `llms.txt` files; if neither works, it states that the API details are unverified.

## Needs
- **Node and `npx`.** The MCP server is declared in `plugins/nexus-mui/.mcp.json` and started with `npx -y @mui/mcp@0.1.7`. The version is pinned.
- An installed MUI in your project is not required, but the agent adapts to it when present.

## With the nexus skills
`implement-feature` delegates UI work to this agent when the add-on is installed. Without it, `implement-feature` writes the UI itself.
