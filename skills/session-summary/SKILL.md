---
name: session-summary
description: Standard for writing a session summary — the durable record of what a work session did, decided, and left open. Use when capturing or closing out a session, writing up completed work, or when asked what happened in a previous session and the records need to be read or extended.
---

# Session summaries

A session summary is the record of one significant piece of work: what happened, what was
decided, and what's still open. It exists because the reasoning behind a change is the part
that doesn't survive in code, and reconstructing it later costs far more than writing it down
once.

Naming, dating, and archiving rules are in `standards/conventions.md`. This document covers what
goes *in* a summary.

## When to write one

Write one for significant work: a feature, a fix with a non-obvious cause, an architectural
decision, a migration, a spike, a handoff.

Skip it for trivial edits — a typo fix, a dependency bump, a rename with no reasoning behind it.
A directory of records for non-events makes the real ones harder to find.

The test: **would a reader six months from now need this, and could they reconstruct it from the
diff alone?** If the diff tells the whole story, the record adds nothing. If the diff shows
*what* changed but not *why that way*, write it.

## Where it goes

```
session-summaries/YYYY-MM-DD-<slug>.md
```

Use `templates/session-summary-template.md` and keep its six `##` headings verbatim. Tools that
index these records key on heading text, so renaming one breaks references to that section
silently. `###` subheadings are free.

## Writing it

**Fill it from what actually happened.** Real file paths, real command names, real decisions.
A summary assembled from what the work was *supposed* to be is worse than none, because it reads
as evidence while being fiction.

**Never invent a number.** If throughput improved, say by how much and how it was measured. If
it wasn't measured, say that. Mark estimates as estimates. A record's whole value is that it can
be trusted without re-verification.

**`## Decisions` is the section that matters most.** Each decision, the alternative rejected, and
why. Include the ones that felt obvious — those are precisely the ones nobody can reconstruct,
because the reasoning was never written anywhere. If a decision was made *for* you by a
constraint, name the constraint.

**Flag uncertainty in place.** "This assumes the upstream response is always sorted — not
verified" is more useful than silence, and it turns into a real next step.

**Write it close to the work.** A summary written a week later is a reconstruction. The details
that make it worth having — the alternative you almost took, the thing that surprised you — are
the first to go.

## Records are append-only

Once written, a summary is not revised to reflect what you later learned. If it turns out to be
wrong, write the correction into the next record.

A summary quietly edited months later stops being evidence of anything, because its value comes
entirely from having been written close to the work. This is the opposite of how documents in
`docs/` work, where content is maintained and marked superseded — records are history, documents
are current state.

## What not to do

- **Don't restate the diff.** A list of changed lines is not a record. What the change *means*
  and why it took that shape is.
- **Don't editorialize about effort.** "This was a difficult refactor" tells a future reader
  nothing actionable. What made it difficult does.
- **Don't leave sections silently empty.** Write "None." so the absence is a statement rather
  than an apparent oversight.
- **Don't split one piece of work across records** to make the directory look busier, or merge
  two unrelated pieces into one record because they shared a day. One topic per file.
