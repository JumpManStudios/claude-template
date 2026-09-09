  # Plans

Working artifacts from plan mode: how to approach something, worked out before doing it.

**Everything in here is transient.** The directory is gitignored except for this README, so plans
are yours alone and disappear with the checkout. That's deliberate — a plan's job ends when the
work lands.

## Files

```
plans/YYYY-MM-DD-<slug>.md
```

Naming and lifecycle rules: `standards/conventions.md`.

## When a plan is done

Delete it, or **graduate** it: rewrite the parts still worth keeping as a `docs/` entry, then
delete the plan. Don't move the file — a plan and a guide have different shapes, and relocating one
produces a document that reads like neither.

A `plans/` directory full of stale files means the graduation step is being skipped.

## Not sure whether something belongs here or in `docs/`?

Ask whether anyone would read it after the work ships. Planning notes for merged work are noise;
the design rationale behind that work is worth keeping. The full rule is the transient/durable
table in `standards/conventions.md`.
