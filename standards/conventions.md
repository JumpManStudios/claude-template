# Conventions

Naming, lifecycle, and archiving rules for everything this template writes. Stated here once and
referenced elsewhere — a rule that appears in two places will drift, so directory READMEs point
here rather than restating.

---

## Record naming

```
YYYY-MM-DD-<slug>.md
```

- **Date first**, ISO order, so files sort chronologically without tooling.
- **Slug describes the work**, not the sequence. `2026-08-13-header-aware-chunking.md`, never
  `session-1.md` or `notes.md`. You will search these by what they were about.
- **Lowercase, hyphen-separated.** No spaces, underscores, or capitals — these get referenced from
  shell commands and URLs.
- **One topic per file.** Two unrelated pieces of work in one session are two records.

Weekly rollups use `week-of-YYYY-MM-DD.md`, dated to the **Monday** of that week regardless of when
the rollup is written.

---

## Transient vs. durable

This is the only directory boundary that requires judgment, so it is the only one stated as a rule:

| | Transient | Durable |
|---|---|---|
| **Where** | `plans/` | `docs/` |
| **Git** | Ignored | Committed |
| **Lifespan** | Delete once the work lands | Kept indefinitely |
| **Purpose** | Working out *how* to do something | Recording what something *is*, or *why* it is |

If you're unsure, ask whether anyone would read it after the work ships. Planning notes for a task
that's already merged are noise; the design rationale behind that task is worth keeping.

A plan is not a lesser document — it's a different one with a shorter life. Plans that turn out to
have lasting value **graduate**: rewrite the durable parts as a `docs/` entry, then delete the plan.
Don't move the plan file itself. A plan and a guide have different shapes, and relocating one
produces a document that reads like neither.

---

## `docs/` frontmatter

Every file in `docs/` carries frontmatter, so the directory stays navigable without subdirectories:

```yaml
---
type: guide | analysis
title: Short descriptive title
date: YYYY-MM-DD
status: current | superseded
---
```

- **`type: guide`** — how to build or operate something. Prescriptive; a reader follows it.
- **`type: analysis`** — an investigation and its conclusion. Descriptive; a reader learns from it.
- **`status: superseded`** — kept for the record but no longer true. Name what replaced it in the
  first line. Deleting a superseded document destroys the reasoning trail; leaving it unmarked
  actively misleads.

The `type:` field replaces what would otherwise be separate directories. Two documents that differ
only in shape don't need separate homes — they need a label.

---

## Records are append-only

Session and weekly summaries describe what happened at a point in time. Once written, they are not
revised to reflect what you later learned.

If a record turns out to be wrong, write the correction into the *next* record. A summary quietly
edited months later is no longer evidence of anything — its value comes entirely from having been
written close to the work.

This is the opposite of the `docs/` rule, where documents are maintained and superseded. Records
are history; documents are current state.

---

## Archiving

- **Session summaries** accumulate. Don't prune them — they're the raw material rollups are built
  from, and they make questions like "when did we change approach, and why" answerable a year later.
- **Weekly summaries** roll up their week's completed work, after which the source list is cleared.
  The rollup becomes the durable form.
- **Plans** are deleted once the work lands, or graduated per above. A stale `plans/` directory means
  the graduation step is being skipped.

Archive by year (`session-summaries/2026/`) only once a flat listing becomes unwieldy. A few dozen
files don't need a hierarchy, and premature nesting makes globbing worse.

---

## Placeholders

Anything an adopter must supply is marked `{{PLACEHOLDER}}` — double braces, `SCREAMING_SNAKE` or
short prose inside, never a plausible-looking fake value.

A fake value that reads as real is the worst outcome: it survives review, ships, and misleads. An
unfilled `{{BUILD_COMMAND}}` is obviously incomplete. A wrong-but-plausible `npm run build` is not.

Before publishing, every `{{PLACEHOLDER}}` should be either filled or deliberately left — never
accidentally left.

---

## Writing style

These documents are read by both people and assistants, and both are served by the same things:

- **Specify, don't persuade.** State the rule and move on. A standard that spends half its length
  arguing for itself is harder to follow and no more likely to be followed.
- **Lead with the rule**, then the rationale if it isn't self-evident. Not the reverse.
- **Concrete over abstract.** `YYYY-MM-DD-<slug>.md` beats "use a consistent naming scheme."
- **No status decoration.** Emoji severity markers, `CRITICAL:`, and bolded warnings stop carrying
  signal once everything has them.
