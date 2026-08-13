---
description: Capture this work session as a structured record in session-summaries/ — what was done, what was decided and why, and what's still open
argument-hint: [short topic, e.g. "header-aware chunking"] (optional)
allowed-tools: Read, Write, Glob, Grep, Bash
---

Write a **session summary** for the work in this session. The reasoning behind a change is the
part that doesn't survive in code — this is where it goes.

**Topic:** `$ARGUMENTS` — a short phrase for the title and slug. If none given, infer it from
what this session actually did.

## Steps

1. Follow the `session-summary` skill for what belongs in each section, and
   `templates/session-summary-template.md` for the shape. Keep the six `##` headings verbatim —
   indexing keys on the heading text.
2. Write to `session-summaries/YYYY-MM-DD-<slug>.md` using today's date (`date +%Y-%m-%d`) and a
   descriptive slug, not `session-1`. Create the directory if it doesn't exist.
3. Fill it from what actually happened in this session — real paths, real decisions, real next
   steps. Read the diff (`git diff`, `git status`) rather than working from memory of intent.
4. Numbers only where they're real and measured. Mark estimates as estimates; never invent one.
5. Give `## Decisions` the most care. Each choice, the alternative rejected, and why — including
   the choices that felt obvious at the time.

Only write records for significant work. If the diff tells the whole story on its own, say so and
skip it rather than producing a record for a non-event.

Don't revise an existing summary to reflect what was learned later — records are append-only, so
corrections belong in the next one.
