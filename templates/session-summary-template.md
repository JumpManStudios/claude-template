# Session Summary: {{SHORT_TITLE}}

**Date:** {{YYYY-MM-DD}}
**Scope:** {{ONE_LINE: what this session set out to do}}

<!--
  The six `##` headings below are fixed. Don't rename, reorder, or remove them — tools that
  index these records key on the heading text, so a rename silently breaks every reference to
  a section. `###` subheadings are yours: add, drop, and rename freely.

  Delete a section's placeholder and write "None." if it genuinely doesn't apply. An empty
  section is information; a deleted one looks like an oversight.
-->

## What I did

{{WHAT_HAPPENED: the work itself, in enough detail that you could resume from cold. Name real
files, real functions, real commands. Prefer specifics over summary — "extracted the retry
logic out of the request handler into its own module" beats "refactored error handling".}}

## Decisions

{{DECISIONS_AND_WHY: each choice, the alternative you rejected, and the reason. This is the
section that pays for the whole record — code shows what you chose and never why. Include
decisions that felt obvious at the time; those are the ones nobody can reconstruct later.}}

## Files changed

{{PATHS, grouped if it helps. Include the shape of the change, not just the name:

- `path/to/file` — created; {{what it does}}
- `path/to/file` — modified; {{what changed and why}}
- `path/to/file` — deleted; {{what replaced it}}
}}

## Impact

{{WHAT_IS_NOW_TRUE_THAT_WASN'T: behavior, capability, performance, or risk removed. Numbers
only where they're real and measured — mark anything estimated as estimated, and never invent
one. An honest "not measured" is worth more than a plausible figure.}}

## Next steps

{{WHAT_COMES_NEXT, concrete enough to act on without rereading the whole record. Include
anything deliberately left undone and why, so a future reader can tell a decision from an
oversight.}}

## Open questions

{{UNRESOLVED: things you don't know, decisions deferred, assumptions not yet verified. Write
"None." if there genuinely are none — but check first, since a session with no open questions
is rarer than it feels.}}
