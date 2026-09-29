---
description: Drive the amux server — board cards (AMUX-*), workers, schedules, memory, diagnostics. Usage: /amux <family> [args...]
---

Load the `amux-skill` skill first (skill tool, name: `amux-skill`), then handle the
request below against the amux HTTP API using `curl -sk` with
`Authorization: Bearer $AMUX_AUTH_TOKEN`.

Request: **$ARGUMENTS**

Rules:

- If `$AMUX_URL` or `$AMUX_AUTH_TOKEN` is unset, ask the user for it before
  calling anything — never guess a token, never print it.
- Empty arguments or `help` → list the families with one example call each:
  `board`, `projects` (create project / submit outcome), `workers`/`sessions`,
  `memory`, `schedules`, `notes` (`/api/memories`), `health`, `diagnostics`.
- Otherwise parse family + verbs per the skill, execute, and report the
  result concisely (card id + status, response body, or the exact error).
  Core families are documented in the skill itself; email, Gmail, calendar,
  Telegram, browser, CRM, journal, files/fs, org, groups, torrents, graph,
  map, SQL, dictation, TTS, habits, review and connectors have per-route
  examples in `references/long-tail.md` inside the same skill directory.
- On `401` stop and fix auth; on `409 gate_blocked` follow the gate rules in
  the skill; on `404` consult `GET /api/debug/routes` instead of guessing
  routes.
- Creating/updating a project (`PUT /api/projects/...`, `/projects/draft`) is
  operator-only: send NO `X-Amux-Worker` header on those calls (403 otherwise).
