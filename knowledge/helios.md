# Helios — Browser Automation for AI Agents

> **Repo:** `eidos-agi/helios` (private)
> **Status:** Functional, actively used
> **Role:** The "body" pillar of Eidos — how agents interact with the web

## What It Is

Helios controls your **actual logged-in browser** — not a headless instance. Agents use your real sessions, cookies, and authenticated state. This is the key difference from Puppeteer/Playwright.

## Architecture

```
Claude Code → MCP Server → Hub (daemon) → Native Host → Chrome Extension → Your Browser
```

- **Chrome Extension** (Manifest V3) — executes actions via `chrome.*` APIs
- **Native Host** — bridge between extension and MCP server via native messaging
- **MCP Server** — translates MCP tool calls into Helios messages
- **Hub** (planned) — central daemon for routing, persona management, session isolation

## Key Concepts

| Concept | What |
|---------|------|
| **Personas** | Named identity profiles (e.g., `cockpit`, `research`) that scope which services an agent can access |
| **Orbits** | Chrome tab groups per agent session — isolation so multiple agents don't stomp each other |
| **Site Knowledge** | Per-domain pattern learning stored in `~/.helios/sites/` — agents learn web quirks over time |
| **Guides** | General automation patterns in `~/.helios/guides/` — searchable knowledge base |

## Monorepo Structure

```
packages/
├── extension/      # Chrome extension (Manifest V3)
├── native-host/    # Native messaging bridge (TypeScript)
├── server/         # MCP server (TypeScript)
└── shared/         # Shared types
```

## Why It Matters for Eidos

Helios IS the agent's body. Without it, agents are text-in/text-out. With it, agents can:
- Log into services using your authenticated sessions
- Read and interact with any web page
- Manage email, project boards, dashboards — anything in a browser
- Learn site patterns that persist across sessions

This directly enables the product vision: "Works with ANY website via screen-and-keyboard control."

## Roadmap (from repo)

- **Multi-session support** — tab groups per MCP client, session broker, concurrent agent isolation
- **Hub daemon** — central router that outlives all connections, manages personas and bindings
- **Learning gateway** — hub remembers which automation method works per domain, self-healing

## Origin

Originally authored under "Jetta Intelligence" — brought into the Eidos AGI org.
