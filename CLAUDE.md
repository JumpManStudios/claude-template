<!--
  This file is loaded into context at the start of every session, so every line is a
  permanent cost paid on every turn — including turns it has nothing to do with.

  Keep it under ~120 lines. It holds facts that are true regardless of what you're
  working on. Anything situational (how to write a summary, how to run a review) belongs
  in skills/, which load only when relevant.

  If this file starts growing sections describing multi-step processes, that's the signal
  a skill is missing — not that the limit needs raising.
-->

# generic-project-for-coding-agent-workspace

A minimal, non-code scaffold for starting a new project. It holds the project's own
`CLAUDE.md` and onboarding docs at the root, and clones the reusable `coding-agent-workspace`
template into `.claude/` as an independently-versioned, gitignored nested repo.

## Commands

There is no application here — nothing to install, build, run, or test. The one action this
repo exists to perform is bootstrapping the workspace:

| Purpose | Command |
|---|---|
| Add the coding agent workspace | `git clone https://github.com/JumpManStudios/coding-agent-workspace .claude` — see `generic-project-setup/add-coding-agent-workspace.md` |

## Architecture

Two independent pieces, deliberately not merged:

- **Root** — the actual project. `CLAUDE.md` (this file) plus `generic-project-setup/`, which
  documents how to wire the workspace into a fresh clone of this scaffold.
- **`.claude/`** — a nested clone of `coding-agent-workspace`, with its own `.git`. Gitignored
  from this repo, so it never appears in `git status`/`git add` at the root. Claude Code
  auto-discovers commands, skills, and settings from `.claude/` with no extra wiring.

| Path | What lives there |
|---|---|
| `CLAUDE.md` | Project instructions Claude reads every session (this file). |
| `generic-project-setup/` | Onboarding doc(s) for wiring `.claude/` into a fresh project. |
| `.claude/` | Nested clone of `coding-agent-workspace` — commands, skills, standards, templates, session summaries. |

## Conventions

- `.claude/CLAUDE.md` is a synced copy of this file, not an `@import` — the two are kept
  byte-identical by hand. Edit here first, then copy this file over `.claude/CLAUDE.md` and
  commit it on `.claude/`'s own branch. If they ever drift, this root copy wins, since that's
  what Claude Code actually reads.

### Known traps

- `.claude/` is **not** disconnected from `origin` the way `.claude/SETUP.md`'s "gitignored
  clone" mode recommends — it's still `https://github.com/JumpManStudios/coding-agent-workspace`,
  just checked out on a project-specific branch (`generic-project-claude`) instead of `main`.
  Never commit project-specific content while on `main` inside `.claude/`.
- The root `.gitignore` excludes `/.claude/` on purpose — its absence from `git status` at the
  root is expected, not a sign something's missing.

## Environment

- Platform: macOS (Darwin 25.6.0)
- Shell: zsh
- Runtime versions: N/A — git and a POSIX shell are the only dependencies.

## Working agreements

- **Conventions for records and documents:** `.claude/standards/conventions.md`. File naming,
  the transient/durable split, `docs/` frontmatter, and archiving live there, not here.
- **Skills:** `.claude/skills/` holds the process standards. They load when the work is
  relevant, so don't restate them in this file.
- **Plans** are transient and gitignored; **`.claude/docs/`** is durable and committed.
- **Records are append-only.** Corrections go in the next record, not by editing an old one.

## Do not put in this file

- Step-by-step processes — those are skills.
- Anything already obvious from the code. This file is for what the code can't tell you.
- Rules stated in `standards/conventions.md`. One home per rule.
- Rituals tied to a day or cadence. They cost context on every unrelated turn.
