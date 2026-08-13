# Session summaries

One record per significant piece of work: what happened, what was decided, and what's still open.

These are the workhorse of this template. Everything else — weekly rollups, status talking points,
picking up where you left off — reads from here.

## Files

```
session-summaries/YYYY-MM-DD-<slug>.md
```

Naming, dating, and archiving rules: `standards/conventions.md`.
What belongs in each section: the `session-summary` skill.
Shape to fill in: `templates/session-summary-template.md`.

Write one with `/session-summary`.

## The six sections

Fixed `##` headings, in this order. Indexing tools key on the heading text, so renaming one
breaks references to that section silently. `###` subheadings are yours.

| Section | Holds |
|---|---|
| `What I did` | The work itself, specific enough to resume from cold. |
| `Decisions` | Each choice, the alternative rejected, and why. |
| `Files changed` | Paths, with the shape of each change. |
| `Impact` | What's now true that wasn't. Real numbers only. |
| `Next steps` | Concrete enough to act on, including what was deliberately left. |
| `Open questions` | Unresolved, deferred, or unverified. |

## These are committed, and append-only

Records are tracked in git — that's the point, and why this directory isn't gitignored the way
`plans/` is.

Once written, a summary isn't revised to reflect what you later learned. Corrections go in the
next record. A summary quietly edited months later stops being evidence of anything, since its
value comes from having been written close to the work.

## Worked example

`examples/session-summary-example.md` is a real record of real work in a public repository, so
every path and decision in it can be checked against the actual commit. Read it before writing
your first one — the shape is easier to copy than to describe.
