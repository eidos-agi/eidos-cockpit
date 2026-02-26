# /land — End of Flight

## When to Use
End of every session. This is the last thing you run.

## What It Does
Lands the plane: cleans up loose ends, conducts a brief debrief interview with the pilot, generates persistent artifacts (`landing.md` + `landing.html`), writes a bookmark for next session, updates state, opens the landing report, and exits cleanly.

## Invocation Modes

- **`/land`** — Full landing. Clean up, interview, artifacts, done.
- **`/land <debrief>`** — Scripted. Uses the provided text as the debrief, skips interview. Still cleans up and generates artifacts.
- **`/land clean`** — Silent. Infers everything from git log and session context. No interview. Still cleans up and generates artifacts.

## Execution Steps

### 1. Gather Session State

**A. Git state** — run `git status --porcelain`, `git log --oneline -5`, `git diff --stat`
- Current branch
- Dirty files (list them)
- Recent commit hashes + messages (to understand what happened this session)
- Diff summary of uncommitted changes

**B. Read state.json** — get current counters, watermarks, theme, cockpit name

**C. Pilot identity** — run `git config user.name`

**D. Session context** — review what happened during this session from conversation history:
- What tools were used
- What files were created or modified
- What decisions were made
- What problems were encountered

### 2. Clean Up

Before the debrief, handle housekeeping:

**A. Uncommitted work** — If there are dirty files:
- Show the user what's uncommitted
- Ask: "Want me to commit these before landing? (y/n/skip)"
- If yes: stage and commit with a descriptive message
- If no/skip: note them as loose ends in the landing report

**B. Loose artifacts** — Check for any temporary files, scratch work, or debug output that should be cleaned up.

**C. Session notes** — If `pilots/<pilot>/session.md` exists and is empty or stale, note it.

### 3. Debrief Interview

**If arguments were provided:** Use them as the debrief. Skip to step 4.

**If "clean" was provided:** Infer the debrief from git log, dirty files, and session context. Skip to step 4.

**Otherwise:** Conduct a brief, conversational debrief. This is NOT a cold checklist — it's a pilot interview. Ask naturally based on what you observed during the session:

Frame it around what you already know. For example:
```
Before I close the logbook — a few loose ends:

1. We [did X, Y, Z] this session. Anything I'm missing or that
   didn't land the way you wanted?

2. [Specific thing that seemed unfinished or uncertain] — is that
   parked intentionally, or should it be top of the list next time?

3. How are you feeling about where things stand? (high / medium / low confidence)
```

**Rules for the interview:**
- Maximum 3 questions, asked all at once
- Reference specific things from the session — don't be generic
- If something looked unfinished, ask about it specifically
- Wait for the user's response before proceeding

### 4. Compile Landing Report

From the session state, cleanup results, and debrief, compile:

| Field | Source |
|---|---|
| **Accomplishments** | What was done (from git log, file changes, session context) |
| **Decisions made** | Any decisions committed to `decisions/` or discussed |
| **Loose ends** | Uncommitted work, unfinished tasks, open questions |
| **Next actions** | Top 3 priorities for next session (from debrief + context) |
| **Blockers** | Anything blocking progress |
| **Confidence** | Pilot's confidence level + risk areas |
| **Lifecycle state** | `done`, `paused`, or `blocked` |

### 5. Write Bookmark

Write to `~/.claude/bookmarks/<session-id>-bookmark.json`:

```json
{
  "schema_version": "1.1",
  "session_id": "<current session ID>",
  "timestamp": "<ISO 8601 UTC>",
  "lifecycle_state": "<done|paused|blocked>",
  "project": {
    "path": "<current working directory>",
    "git_branch": "<current branch>"
  },
  "context": {
    "summary": "<accomplishments, condensed to 1-2 sentences>",
    "goal": "<what we were trying to do this session>",
    "current_step": "<where we stopped>"
  },
  "workspace_state": {
    "dirty": <true|false>,
    "uncommitted_files": [<list of dirty files>],
    "last_commit": "<hash> <message>",
    "diff_summary": "<git diff --stat output>"
  },
  "next_actions": [
    "<action 1>",
    "<action 2>",
    "<action 3>"
  ],
  "blockers": [<blockers if lifecycle_state is "blocked">],
  "confidence": {
    "level": "<high|medium|low>",
    "risk_areas": [<any risks mentioned>]
  },
  "meta": {
    "created_by": "manual"
  }
}
```

### 6. Generate landing.md

Write a markdown file at the cockpit root: `landing.md`

This is the persistent, git-friendly landing report. It mirrors takeoff.md in structure and detail.

Format:

```markdown
# <Cockpit Name> — Landing #N

**Pilot** <name> &nbsp;|&nbsp; **Date** Feb 25, 2026 &nbsp;|&nbsp; **Time** 11:30 PM

**Session** #N &nbsp;|&nbsp; **Branch** `main` &nbsp;|&nbsp; **State** done / paused / blocked &nbsp;|&nbsp; **Confidence** high / medium / low

---

## What We Accomplished

<Full description of what was done. Be specific — reference files, decisions, commits.>

## Decisions Made

<Any decisions from this session, or "None — planning/execution session.">

## Loose Ends

<Uncommitted work, unfinished tasks, open questions. Or "All clean.">

## Next Actions

1. <priority — with WHY it matters>
2. <priority>
3. <priority>

## Blockers

<Blocker details, or "None.">

---

*Generated <ISO timestamp> by /land*
```

**The markdown version should be thorough.** This is what the next session's /takeoff will read to understand where we left off.

### 7. Generate landing.html

Read the HTML template at `.claude/skills/land/landing-template.html`.

Replace all `{{PLACEHOLDER}}` tokens with actual values:

| Placeholder | Source |
|---|---|
| `{{COCKPIT_NAME}}` | `state.json → cockpit.name` |
| `{{PRIMARY}}` | `state.json → theme.primary` |
| `{{DANGER}}` | `state.json → theme.danger` |
| `{{WARNING}}` | `state.json → theme.warning` |
| `{{BG}}` | `state.json → theme.bg` |
| `{{SURFACE}}` | `state.json → theme.surface` |
| `{{TEXT}}` | `state.json → theme.text` |
| `{{MUTED}}` | `state.json → theme.muted` |
| `{{PILOT}}` | `git config user.name` |
| `{{DATE}}` | Human-readable date |
| `{{TIME}}` | Human-readable time |
| `{{LANDING_NUMBER}}` | `counters.landings + 1` |
| `{{SESSION_NUMBER}}` | `counters.sessions` (current, not +1) |
| `{{BRANCH}}` | Current git branch |
| `{{DIRTY_CLASS}}` | `clean` or `dirty` |
| `{{DIRTY_VALUE}}` | `Clean` or `N files modified` |
| `{{STATE_CLASS}}` | `done`, `paused`, or `blocked` (CSS class) |
| `{{STATE_VALUE}}` | `Done`, `Paused`, or `Blocked` |
| `{{CONFIDENCE_CLASS}}` | `high`, `medium`, or `low` |
| `{{CONFIDENCE_VALUE}}` | `High`, `Medium`, or `Low` |
| `{{ACCOMPLISHMENTS}}` | Briefing content wrapped in `<p>` tags |
| `{{DECISIONS}}` | Content wrapped in `<p>` tags or `<ul><li>` |
| `{{LOOSE_ENDS}}` | Content wrapped in `<p>` tags or `<ul><li>` |
| `{{NEXT_ACTIONS}}` | Content as `<ol><li>` items |
| `{{BLOCKERS}}` | Content wrapped in `<p>` tags |
| `{{TIMESTAMP}}` | Human-readable date/time |

Write the result to `landing.html` at the cockpit root.

### 8. Open Landing Report

Open the landing report in the browser:
```bash
open landing.html
```

### 9. Update State

Write to `state.json`:
- Set `watermarks.last_land` to current ISO timestamp
- Increment `counters.landings` by 1

### 10. Confirm Landing

Output ASCII art header:

**Line 1: Cockpit name** — Generate the `cockpit.name` from state.json as bold Unicode block letters (same style as LAND below). Dynamic per cockpit.

**Line 2: LAND** — Always this exact text:

```
  ██╗      █████╗ ███╗   ██╗██████╗
  ██║     ██╔══██╗████╗  ██║██╔══██╗
  ██║     ███████║██╔██╗ ██║██║  ██║
  ██║     ██╔══██║██║╚██╗██║██║  ██║
  ███████╗██║  ██║██║ ╚████║██████╔╝
  ╚══════╝╚═╝  ╚═╝╚═╝  ╚═══╝╚═════╝
```

**Then the status block:**
```
  STATE     <lifecycle_state>
  SUMMARY   <1-line summary>
  NEXT      <top priority for next session>
  COCKPIT   landing.md + landing.html written → opened in browser

  Bookmark written. Safe to close.
```

### 11. STOP

Do NOT proceed with any further work. The session is over.

## Rules
- The interview should feel conversational, not bureaucratic. Reference specific things from the session.
- If the user says "just land", "clean", or gives a terse response, infer what you can and don't push.
- If the user doesn't respond to the interview, write the bookmark with what you know. Use `confidence.level: "low"` and note "Auto-inferred from session context" in the summary.
- Always write the bookmark. Even a low-confidence bookmark is better than nothing.
- Always generate both landing.md and landing.html. These are the session's permanent record.
- Always open landing.html at the end.
- `landing.md` and `landing.html` are overwritten on every landing. They reflect the current session's ending position.
- lifecycle_state:
  - `done` — session goal was completed
  - `paused` — work in progress, stopping by choice
  - `blocked` — can't continue without external input
- If `theme` is missing from state.json, use defaults: primary `#3fb950`, danger `#f85149`, warning `#d29922`, bg `#0d1117`, surface `#161b22`, text `#e6edf3`, muted `#8b949e`.
