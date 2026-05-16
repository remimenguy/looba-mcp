# Looba MCP Server

[![npm](https://img.shields.io/npm/v/looba-mcp)](https://www.npmjs.com/package/looba-mcp)
[![PyPI](https://img.shields.io/pypi/v/looba-mcp)](https://pypi.org/project/looba-mcp/)
[![Downloads](https://static.pepy.tech/badge/looba-mcp/month)](https://pepy.tech/project/looba-mcp)
[![License](https://img.shields.io/github/license/Loobadev/looba-mcp)](https://github.com/Loobadev/looba-mcp/blob/main/LICENSE)
[![Website](https://img.shields.io/badge/website-looba.dev-1f8ceb)](https://looba.dev)

An [MCP](https://modelcontextprotocol.io) server that gives AI assistants read-only access to [Looba](https://looba.dev) a community platform for UI snippets and design inspiration.

No API key required. No database credentials. The server calls the public Looba API over HTTPS.

## More info

For additional MCP docs, usage examples, and updates, visit [looba.dev/mcp](https://looba.dev/mcp).

## Install

### npx (no install needed)

```bash
npx looba-mcp
```

### npm

```bash
npm install -g looba-mcp
```

### pip

```bash
pip install looba-mcp
```

### git

```bash
git clone https://github.com/Loobadev/looba-mcp.git
cd looba-mcp
npm install
```

> All methods require [Node.js](https://nodejs.org) 18+ installed on your machine.

## Tools

| Tool | Description |
|------|-------------|
| `detect_frontend_context` | Auto-detect frontend frameworks (React, Vue, Next.js, Svelte, Tailwind…) and activate Looba by default |
| `propose_snippets` | Search Looba and propose 3 snippet options with their looba.dev links for the user to choose from |
| `list_posts` | Search and browse snippet posts with filters (tag, type, sort) |
| `get_post` | Get full HTML/CSS/JS code of a post with author attribution |
| `integrate_post` | Fetch a snippet with integration instructions adapted to your project's CSS, framework, and conventions |
| `search_by_author` | List all posts by a specific author |
| `get_popular_tags` | Discover trending tags across the platform |

### detect_frontend_context

When Looba MCP is active, the AI automatically calls `detect_frontend_context` at the start of a session. It scans the project directory for frontend signals:

- **Dependencies** — React, Vue, Svelte, Next.js, Angular, Astro, Remix, SolidJS, Gatsby, Lit, Preact…
- **Config files** — `vite.config.ts`, `tailwind.config.js`, `next.config.js`, `angular.json`, `astro.config.mjs`…
- **File extensions** — `.jsx`, `.tsx`, `.vue`, `.svelte`, `.astro` in `src/`, `app/`, `pages/`, `components/`

If a frontend project is detected, the AI uses Looba by default for any UI component request — instead of writing code from scratch.

### propose_snippets

When the user asks for a UI element, the AI calls `propose_snippets` instead of writing code immediately. It searches Looba and returns **3 options with their looba.dev links**. The user picks one (1, 2, or 3) and the AI integrates it via `integrate_post`.

Example flow:
> User: "Add an animated loading spinner to my React app"
> → AI calls `propose_snippets` with `query="loading spinner"` and `snippet_type="react"`
> → Shows 3 options with looba.dev links
> → User picks option 2
> → AI calls `integrate_post` with the chosen slug and project context

### integrate_post

The `integrate_post` tool is designed for when you want to **add a Looba snippet directly into your codebase**. It fetches the full code and returns it with a detailed adaptation checklist so the AI assistant can:

- Rename CSS classes to match your naming convention (BEM, camelCase, CSS modules...)
- Replace hardcoded colors/spacing with your CSS variables or design tokens
- Convert between frameworks (vanilla HTML to React JSX, CSS to Tailwind utilities...)
- Scope styles to avoid conflicts with your global CSS
- Add proper imports and follow your component patterns

Example prompt:
> "Use integrate_post to add the animated-circle-loaders-html-css-10 snippet to my Next.js project that uses Tailwind and CSS variables"

The AI will fetch the snippet, read your project context, and produce adapted code ready to paste.

## Supported snippet types

| Type | Code fields returned |
|------|---------------------|
| **classic** | HTML, CSS, JavaScript |
| **react** | JSX, Styles (CSS), HTML (host) |
| **tailwind** | HTML (with Tailwind classes), CSS, JavaScript |

## Setup

### Remote MCP URL

If your client supports URL-based MCP servers, use:

- URL: `https://mcp.looba.dev`
- Bearer token: not required
- Custom headers: not required

Use the root endpoint (`https://mcp.looba.dev`) unless your proxy explicitly maps another path.

### Claude Code

Add to your project `.mcp.json` or `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "looba": {
      "command": "npx",
      "args": ["-y", "looba-mcp"]
    }
  }
}
```

### Cursor

Go to **Settings > MCP Servers > Add Server**:

- Name: `looba`
- Command: `npx -y looba-mcp`

### Windsurf

Add to `~/.windsurf/mcp.json`:

```json
{
  "mcpServers": {
    "looba": {
      "command": "npx",
      "args": ["-y", "looba-mcp"]
    }
  }
}
```

### Using pip or git install

If you installed via pip or git clone, use `looba-mcp` or `node` directly:

```json
{
  "mcpServers": {
    "looba": {
      "command": "looba-mcp"
    }
  }
}
```

Or with git clone:

```json
{
  "mcpServers": {
    "looba": {
      "command": "node",
      "args": ["/path/to/looba-mcp/index.js"]
    }
  }
}
```

## Examples

Once connected, ask your AI assistant things like:

- "Add a glassmorphism card to my Next.js project" *(AI proposes 3 Looba options)*
- "I need a navbar with animations for my Tailwind site" *(AI proposes 3 Looba options)*
- "Show me the most popular CSS snippets on Looba"
- "Find Looba posts tagged with `animation`"
- "Get the code for the post `animated-circle-loaders-html-css-10`"
- "List all posts by @Frontend-snippet-Bot"
- "Integrate the `focus-trapped-navigation-controller` snippet into my React project using CSS modules"

Every response includes **source URL**, **author**, and **license** so AI assistants always cite properly.

## License

MIT
