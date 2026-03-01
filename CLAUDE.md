# CLAUDE.md — Eidos Cockpit

> **Multi-pilot planning, strategy, and mission control for Eidos AGI.**

## Role Context

This is the operational cockpit for **Eidos** at **Eidos AGI**. It serves as the planning hub for the entire Eidos ecosystem — product vision, architecture decisions, sprint planning, and cross-pilot coordination.

**Pilots**: Daniel, Vybhav, and any future contributors (human or AI).

## Multi-Pilot Protocol

This cockpit supports multiple pilots working simultaneously. The rules:

1. **Per-pilot state** — Each pilot has their own directory under `pilots/<name>/`. Your session logs, notes, and scratch work go here. These rarely conflict on merge.
2. **Shared knowledge** — `knowledge/` is append-only. Add new files, don't edit others' files mid-flight. Merge-friendly by design.
3. **Shared decisions** — `decisions/` is append-only. One file per decision, named with date prefix. Never conflicts.
4. **Backlog.md** — Single source of truth for tasks. Backlog MCP handles merge conflicts via structured edits.
5. **Fetch before takeoff** — Always `git pull` before `/takeoff`. Two pilots merge into the same plane, not fork into two.

## Cockpit Primitives

| Skill | When | What |
|-------|------|------|
| `/takeoff` | Start of session | Boot: pull latest, read bookmark, drift check, show priorities |
| `/land` | End of session | Capture outcomes, write bookmark, push |
| `/cockpit-status` | Anytime | Active workstreams, blockers, pilot activity |
| `/cockpit-repair` | When things break | Validate state, find corruption, offer fixes |

## Session Protocol

1. `git pull` (merge other pilots' work into your plane)
2. `/takeoff` (boot sequence)
3. Work — plan, research, decide, update backlog
4. `/land` (capture outcomes, push)

If a session crashes without `/land`, the next `/takeoff` will detect drift and flag it.

## Eidos Ecosystem

| Repo | What It Is |
|------|-----------|
| `cockpit-eidos` | This repo — planning, strategy, mission control |
| `cockpit-daniel` | Daniel's personal cockpit — founder ops, email, account management |
| `cockpit-vybhav` | Vybhav's personal cockpit — founder ops, research |
| `eidos-v5` | The engine — pods, PEFM memory, orchestrator, research pipeline |
| `helios` | Browser automation for AI agents — Chrome extension + MCP server, identity-aware |
| `eidos-infra` | Infrastructure and deployment |
| `eidos-coo` | COO agent — manages a fleet of specialist AI employee agents |
| `eidos-rolodex` | Relationships, contacts, and identity management |
| `eidos-philosophy` | Core philosophy and identity documents |
| `eidos-studies` | Research studies and experiments |

## Product Vision (North Star)

**Eidos is a full agent — not a bot.**

- Works with ANY website, ANY terminal, ANY interface via screen-and-keyboard control
- APIs are optimizations; screen control is the universal fallback
- Has its own identity: email, phone, Slack, persistent presence
- Onboards like a human, completes real missions, asks for help when stuck
- Token burn is an acceptable cost for universality

### Three Pillars

| Pillar | What | Repo |
|--------|------|------|
| **Eidos Core** (brain) | PEFM memory, pods, orchestrator, memory compression | `eidos-v5` |
| **Hancock** (auth) | Delegation certificates, approval dashboard, 2FA relay | TBD |
| **Helios** (body) | Browser automation, identity-aware web control, personas, site learning | `helios` |

## Domain Skills

| Skill | What It Does |
|-------|-------------|
| `/plan-sprint` | Review backlog, prioritize, assign work across pilots |
| `/vision-check` | Re-read product philosophy, check alignment of current work |
| `/research-brief` | Summarize research findings for a topic into a decision-ready brief |

## Project Structure

```
cockpit-eidos/
├── CLAUDE.md              ← you are here
├── state.json             ← cockpit state & watermarks
├── pilots/                ← per-pilot state (merge-friendly)
│   ├── daniel/
│   │   └── session.md     ← current session notes
│   └── vybhav/
│       └── session.md
├── knowledge/             ← shared knowledge base (append-only)
│   └── ...
├── decisions/             ← architectural decisions (date-prefixed, append-only)
│   └── ...
├── briefs/                ← research briefs and summaries
│   └── ...
└── .claude/
    └── skills/            ← cockpit skills
```

## Rules

- **Soft deletes only** — never hard-delete records
- **No secrets in repos** — use environment variables or Knox
- **Commit messages explain why**, not just what
- **Append, don't edit** shared files — prevents merge conflicts across pilots
- **Pull before takeoff** — always merge other pilots' work first
- **Decisions are permanent** — once written to `decisions/`, they stand unless a new decision supersedes

<!-- BACKLOG.MD MCP GUIDELINES START -->

<CRITICAL_INSTRUCTION>

## BACKLOG WORKFLOW INSTRUCTIONS

This project uses Backlog.md MCP for all task and project management activities.

**CRITICAL GUIDANCE**

- If your client supports MCP resources, read `backlog://workflow/overview` to understand when and how to use Backlog for this project.
- If your client only supports tools or the above request fails, call `backlog.get_workflow_overview()` tool to load the tool-oriented overview (it lists the matching guide tools).

- **First time working here?** Read the overview resource IMMEDIATELY to learn the workflow
- **Already familiar?** You should have the overview cached ("## Backlog.md Overview (MCP)")
- **When to read it**: BEFORE creating tasks, or when you're unsure whether to track work

These guides cover:
- Decision framework for when to create tasks
- Search-first workflow to avoid duplicates
- Links to detailed guides for task creation, execution, and finalization
- MCP tools reference

You MUST read the overview resource to understand the complete workflow. The information is NOT summarized here.

</CRITICAL_INSTRUCTION>

<!-- BACKLOG.MD MCP GUIDELINES END -->
