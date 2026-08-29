# coding-agent-workspace

An isolated, human-in-the-loop workspace for AI-assisted software development: project guidance,
Agent Skills, session records, planning conventions, and durable documentation practices.

This project began as `claude-template`, extracted from a private Claude Code workspace used in
daily development. The useful part was not Claude-specific configuration; it was the discipline
around preserving decisions, handing work across sessions, and turning agent activity into a
durable record. The repository is now being generalized in place rather than split into separate
host forks.

The project is intentionally optimized for a developer working interactively with local coding
agents. Shared behavior has one canonical home in an isolated nested repository. Ignored root
files and host directories project it through each coding assistant's native discovery paths, so
the product repository stays free of skills, session records, and agent-specific configuration.

Sibling repository: [`fact-bank-resume-builder`](https://github.com/JumpManStudios/fact-bank-resume-builder)
applies the same record-first approach to a job-search knowledge base and MCP workflow.

## Compatibility

Common format support is not the same as tested integration. A host is marked supported only after
its documented installation and a representative workflow pass in a clean project.

| Host | Instructions | Skills | Local-install status |
|---|---|---|---|
| Claude Code | Thin `CLAUDE.md` imports projected `AGENTS.md` | `.claude/skills/` links | Implemented; clean-room verification pending in [#17](https://github.com/JumpManStudios/coding-agent-workspace/issues/17) |
| Codex | Project-root `AGENTS.md` projection | `.agents/skills/` links | Planned in [#18](https://github.com/JumpManStudios/coding-agent-workspace/issues/18) |
| Windsurf | Project-root `AGENTS.md` projection | `.windsurf/skills/` links | Planned in [#19](https://github.com/JumpManStudios/coding-agent-workspace/issues/19) |
| Cursor | `AGENTS.md` | Agent Skills-compatible | Format-compatible; not verified |
| GitHub Copilot | Surface-dependent instruction support | Agent Skills-compatible | Format-compatible; not verified |

The repository can be named or stored anywhere. Installed adapters still have to use the host's
documented discovery paths; a generic directory name does not replace `.claude/`, `.agents/`, or
another host-specific configuration surface.

These statuses describe local, human-in-the-loop installations. Cloud-agent compatibility is a
different consumption model: the necessary instructions and adapters must be committed to the
product repository or installed by its environment. See
[Converting a project for cloud agents](docs/cloud-agent-conversion.md); this project does not
currently claim verified cloud support.

## Why this exists

The hard part of working with a coding agent is not usually the code it writes. It is the loss of
context between sessions: decisions disappear, rejected alternatives are re-explored, and the
reasoning behind a change survives only in somebody's head.

```text
work happens -> capture it as a record -> reuse that record as future context
             -> roll records up       -> answer "what did I actually ship?"
```

The result is useful both to the next agent session and to the human who needs a standup update,
review narrative, handoff, or evidence of completed work.

## Architecture

The repository has three layers:

| Layer | Responsibility |
|---|---|
| Shared core | The nested workspace's `AGENTS.md`, `skills/`, `standards/`, and `templates/` define behavior once. |
| Host adapters | Ignored root files and host directories link the core into native discovery paths; `adapters/` documents the wiring. |
| Workspace outputs | `session-summaries/`, future weekly summaries, `plans/`, and `docs/` hold the records produced while working. |

For the supported local model, relative symlinks are the primary projection mechanism because
edits still land in the nested workspace repository. Copying is a documented fallback where links
are unavailable, never a second source of truth. Adapters must not copy a workflow body merely to
change invocation syntax.

## What's reusable

- **Session-summary discipline:** one structured record per significant piece of work, written
  close to the work and shaped for later retrieval.
- **Agent Skills instead of stuffed instructions:** multi-step workflows load on demand while
  always-on guidance stays short.
- **Transient versus durable outputs:** plans are disposable; guides, analyses, and records are
  retained according to explicit lifecycle rules.
- **A core/adapter boundary:** portable behavior stays independent of host settings, permissions,
  hooks, and tool names.

## Directory map

| Path | What lives there |
|---|---|
| `AGENTS.md` | Canonical, provider-neutral project guidance tracked in the workspace repository. |
| `CLAUDE.md` | Thin Claude entry point for developing this repository; imports `AGENTS.md`. |
| `adapters/` | Host-specific discovery, configuration examples, installation notes, and verification steps. |
| `skills/` | Canonical Agent Skills and their workflow bodies. |
| `standards/conventions.md` | Naming, lifecycle, and archiving rules stated once. |
| `templates/` | Output shapes used by canonical skills. |
| `session-summaries/` | Append-only records for significant work. |
| `plans/` | Transient planning artifacts; gitignored except for its README. |
| `docs/` | Durable guides and analyses, committed and maintained. |
| `examples/` | Worked public examples whose claims can be inspected. |

## Current workflow

`session-summary` is the migration proof for the architecture. The canonical skill owns the full
workflow and writes:

```text
session-summaries/YYYY-MM-DD-<slug>.md
```

When exposed through the Claude adapter, the skill is invoked directly as `/session-summary`.
There is no parallel Claude command body to drift from the portable skill.

## Quickstart

The recommended local layout keeps all agent artifacts in a private nested repository. The root
instruction files and host directories are ignored projections, so editing the `AGENTS.md` symlink
from the product root still changes the file tracked by the workspace repository:

```text
your-project/
├── .agent-workspace/     # nested Git repository: canonical content + private records
├── AGENTS.md             # ignored link -> .agent-workspace/AGENTS.md
├── CLAUDE.md             # ignored thin Claude entry point
├── .claude/              # ignored Claude discovery/settings adapter
└── product source
```

The product repository tracks none of those four paths. Its own status and history remain focused
on product code, while improvements to guidance and records appear in `.agent-workspace` Git.

See [SETUP.md](SETUP.md) for the complete Claude installation, alternate workspace locations,
copy fallback, verification steps, and migration from the former `.claude/` clone layout.
The rationale and rejected alternatives are recorded in
[docs/install-layout-analysis.md](docs/install-layout-analysis.md).

Teams that want fresh clones, CI, or cloud agents to receive the same context can promote selected
instructions and skills into the product repository. That conversion trades isolation for
portability and should keep private session records and machine-local settings excluded. The
[cloud-agent conversion guide](docs/cloud-agent-conversion.md) describes the boundary without
presenting cloud execution as this repository's primary or verified workflow.

## Roadmap

- [ ] [#17](https://github.com/JumpManStudios/coding-agent-workspace/issues/17) — provider-neutral core, Claude adapter, migration, and clean-room verification
- [ ] [#18](https://github.com/JumpManStudios/coding-agent-workspace/issues/18) — Codex adapter and second-host proof
- [ ] [#19](https://github.com/JumpManStudios/coding-agent-workspace/issues/19) — Windsurf adapter and third-host proof
- [ ] Workflow slices — weekly summary, end-task session, standup preparation, and PR description
- [ ] Final package inventory and supported-host publish gate

The project follows one rule as this list grows: **design from documented behavior; claim support
only after clean-room verification.**

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, and reshape it to your own practice.
