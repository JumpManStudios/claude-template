<!--
  This file is the provider-neutral, always-on project guidance tracked by the
  isolated workspace repository and projected to the product root. Keep it short:
  everything here is loaded for every task by hosts that support AGENTS.md, or by
  a thin host adapter such as CLAUDE.md.

  Multi-step processes belong in skills/, which load only when relevant. If this
  file starts describing workflows, create or improve a skill instead.
-->

# {{PROJECT_NAME}}

{{ONE_OR_TWO_SENTENCES: what this project is and who uses it}}

## Commands

| Purpose | Command |
|---|---|
| Install dependencies | `{{INSTALL_COMMAND}}` |
| Run locally | `{{RUN_COMMAND}}` |
| Build | `{{BUILD_COMMAND}}` |
| Test (all) | `{{TEST_COMMAND}}` |
| Test (single file) | `{{TEST_SINGLE_FILE_COMMAND}}` |
| Lint / format | `{{LINT_COMMAND}}` |
| Type check | `{{TYPECHECK_COMMAND}}` |

Run `{{LINT_COMMAND}}` and `{{TEST_COMMAND}}` before declaring work finished.

The single-file test command matters more than it looks: without it the whole suite gets run
to check one change.

## Architecture

{{HOW_THE_PIECES_FIT: the 3-5 sentence version. Where a request enters, what handles it,
where state lives, what talks to what. Enough that a reader knows which directory to open
without exploring.}}

| Path | What lives there |
|---|---|
| `{{PATH}}` | {{WHAT}} |
| `{{PATH}}` | {{WHAT}} |
| `{{PATH}}` | {{WHAT}} |

## Conventions

{{PROJECT_SPECIFIC_RULES. Only things a competent developer would get wrong without being
told — not general good practice. Examples of the shape:
- Which layer is allowed to talk to the database
- The established pattern for a new endpoint, and an existing one to copy
- Error handling: what gets thrown, what gets returned, what gets logged
- Naming that differs from the language default}}

### Patterns to follow

{{POINT_AT_REAL_FILES: "New endpoints follow {{EXAMPLE_FILE}}." A concrete example beats a
description of one.}}

### Known traps

{{THINGS_THAT_LOOK_WRONG_BUT_ARE_DELIBERATE, and things that look fine but break. This
section repays itself faster than any other.}}

## Environment

- Platform: {{OS_AND_VERSION}}
- Shell: {{SHELL}}
- Runtime versions: {{LANGUAGE_AND_VERSION}}, {{PACKAGE_MANAGER_AND_VERSION}}

{{PLATFORM_SPECIFIC_GOTCHAS: path separators, line endings, commands that differ from the
docs, tools not on PATH. Leave empty rather than filling with generic advice.}}

## Coding-agent workspace

- **Workspace root:** the real parent directory of this `AGENTS.md` after resolving any adapter
  symlink. Resolve package paths from there, not from the coding agent's process working directory.
- **Records and document conventions:** `standards/conventions.md`. Naming, lifecycle, the
  transient/durable split, `docs/` frontmatter, and archiving live there.
- **Skills:** `skills/` holds the canonical process standards. Host adapters expose these skills
  through native discovery paths without copying their workflow bodies.
- **Plans** are transient and gitignored; **`docs/`** is durable and committed.
- **Records are append-only.** Corrections go in the next record, not by editing an old one.

## Do not put in this file

- Step-by-step processes — those are skills.
- Host-specific settings, permissions, hooks, workflow syntax, or tool names — those belong in
  `adapters/`.
- Anything already obvious from the code.
- Rules stated in `standards/conventions.md`. One home per rule.
- Rituals tied to a day or cadence. They cost context on every unrelated turn.
