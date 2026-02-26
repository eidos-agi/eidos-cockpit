# Eidos Cockpit

Multi-pilot planning, strategy, and mission control for Eidos AGI.

**Hop in. Fly. Land.**

## What This Is

The operational command post for the Eidos ecosystem. Multiple pilots (Daniel, Vybhav, future contributors) work here simultaneously — planning sprints, making architectural decisions, tracking research, and coordinating across repos.

Built from the [AI Cockpit Template](https://github.com/rhea-impact/ai-cockpit-template).

## Quick Start

```bash
cd eidos-cockpit
git pull                    # merge other pilots' work
claude                      # open Claude Code
# > /takeoff                # boot sequence
# > ... plan, decide, research ...
# > /land                   # park and push
```

## Multi-Pilot Design

Two pilots can work simultaneously without conflicts:

- **`pilots/<name>/`** — Per-pilot scratch space. Your session notes, thinking, TODOs.
- **`knowledge/`** — Shared knowledge base. Append-only (add files, don't edit others').
- **`decisions/`** — Architectural decisions. Date-prefixed, append-only. Permanent record.
- **`briefs/`** — Research summaries and decision-ready briefs.
- **`state.json`** — Cockpit state. Skills manage this via structured updates.
- **Backlog.md** — Task management via MCP. Single source of truth for work items.

**The rule**: fetch/pull before takeoff. Two pilots merge into the same plane, never fork into two.

## Skills

| Skill | Trigger | What It Does |
|-------|---------|-------------|
| `/takeoff` | Start of session | Pull, read bookmark, drift check, show priorities |
| `/land` | End of session | Capture outcomes, write bookmark, push |
| `/cockpit-status` | Anytime | Active workstreams, blockers, pilot activity |
| `/pre-flight` | Called by takeoff | Deep scan: where we were / are / going / blockers |
| `/cockpit-repair` | When things break | Validate state, find corruption, offer fixes |

## Cockpit Architecture

```
eidos-cockpit/
├── .claude/
│   └── skills/            # Session lifecycle + domain skills
├── CLAUDE.md              # Role context + instructions
├── state.json             # Cockpit state & watermarks
├── pilots/                # Per-pilot state (merge-friendly)
│   ├── daniel/
│   └── vybhav/
├── knowledge/             # Shared knowledge (append-only)
├── decisions/             # Architectural decisions (append-only)
├── briefs/                # Research briefs & summaries
└── docs/                  # Guides (from template)
```

## Ecosystem

| Repo | Role |
|------|------|
| **eidos-cockpit** (this) | Planning & mission control |
| **eidos-v5** | Engine — pods, PEFM memory, orchestrator |
| **eidos-infra** | Infrastructure & deployment |
| **eidos-philosophy** | Core identity & philosophy docs |
| **eidos-studies** | Research studies & experiments |

## License

MIT. Built by [Eidos AGI](https://github.com/eidos-agi).
