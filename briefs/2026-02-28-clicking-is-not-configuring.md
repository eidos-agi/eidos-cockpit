# Clicking Is Not Configuring

**Date** 2026-02-28
**Source** Sable cockpit incident — schema exposure wiped by org migration
**Lesson for** Team and execs

---

## What Happened

Sable went dark to every user. The app loaded fine. Login worked. Zero data on every page. No error anywhere. Because a setting that was clicked into a Supabase dashboard six months ago got wiped when the database moved orgs, and there was nothing in the codebase to put it back.

Every observability tool said green. Vercel returned 200. The health check passed. The app was "up" — but the instruments weren't connected.

## The Rule

**If the only record of a configuration is a screenshot or someone's memory, it's already broken — you just don't know it yet.**

When you set something in a dashboard instead of writing it in code, you've created a secret dependency that nobody can see, nobody can review, and nobody can restore. It works until it doesn't — and when it breaks, there's no error message, no alert, and no trail to follow.

## What This Means for Eidos

- **If it's not in a migration, it doesn't exist.** Database schemas, RLS policies, API exposure settings — all in code, all in version control.
- **Dashboard settings are ephemeral.** Treat them like cache: useful for speed, never the source of truth.
- **The cockpit catches it.** `/pre-flight` queries the database through the same path the frontend uses. If schemas disappear — org move, dashboard accident, Supabase upgrade — the next takeoff flags it before any user sees an empty page.

## The Cockpit's Role

Dashboard monitoring tells you the plane is on the runway. The cockpit tells you the instruments are connected.

Without the cockpit, this bug sat silently until someone opened the app and saw nothing. No monitoring caught it. No test failed. The cockpit runs `/pre-flight` at session start and actually tests the real paths. That's the difference.

---

*Learned from sable-cockpit, Feb 28, 2026.*
