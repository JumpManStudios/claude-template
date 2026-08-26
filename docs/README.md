# Docs

Durable documents: what something *is*, or *why* it is. Committed and kept indefinitely.

This is the counterpart to `plans/`. Plans work out how to do something and are deleted once it's
done; docs record what's true and are maintained.

## Files

```
docs/<descriptive-name>.md
```

Every document here — this README aside — carries frontmatter, which is what keeps the directory
navigable without subdirectories:

```yaml
---
type: guide | analysis
title: Short descriptive title
date: YYYY-MM-DD
status: current | superseded
---
```

- **`type: guide`** — how to build or operate something. A reader follows it.
- **`type: analysis`** — an investigation and its conclusion. A reader learns from it.

Field definitions and the superseding rule: `standards/conventions.md`.

## These are maintained, not append-only

Unlike `session-summaries/`, documents here are edited to stay true. When one stops being true,
mark it `status: superseded` and name its replacement in the first line rather than deleting it —
deleting destroys the reasoning trail, and leaving it unmarked actively misleads.
