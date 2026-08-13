# claude-template

A drop-in Claude Code operating layer: slash commands, skills, and session-discipline
conventions for any project.

This is the `.claude/` directory you wish every repo had. Clone it into a project and Claude Code
gains a consistent way to handle the work that surrounds writing code — capturing a session,
drafting a PR description, planning a task, reviewing a diff — plus the written standards that
make those outputs uniform instead of improvised.

It is deliberately **provider-neutral**: nothing assumes a particular issue tracker, CI system, or
language. Where a concrete example helps, examples use GitHub.

Sibling repo: [`fact-bank-resume-builder`](https://github.com/JumpManStudios/fact-bank-resume-builder)
applies the same discipline to a job hunt. The two share conventions deliberately — if you like one,
the other will feel familiar.

## Why this exists

The hard part of working with an AI assistant isn't the code it writes, it's the amnesia. Every
session starts cold. Decisions made on Tuesday are gone by Thursday, the reasoning behind them
lives only in your head, and the assistant re-derives context you already paid for once.

This repo is a bet that the fix is written discipline, not a better prompt:

```
work happens  →  capture it as a record  →  the record is context next session
              →  roll records up weekly  →  you can answer "what did I actually do"
```

Two consequences fall out of that. Your assistant stops starting from zero, and you end up with a
durable account of your own work — useful at review time, in a standup, or on a resume.

## What's genuinely reusable here

- **The session-summary discipline** — the highest-value habit in the repo, and the cheapest. One
  structured record per significant session, in a fixed shape, so it's greppable later and worth
  feeding back as context.
- **Skills over stuffed instructions** — the standards live in `skills/` and load when the work is
  relevant, instead of sitting in `CLAUDE.md` taxing every turn. `CLAUDE.md` stays small on
  purpose.
- **The transient/durable split** — planning artifacts are disposable and gitignored; guides and
  analyses are durable and committed. One boundary, applied consistently, replaces a pile of
  "which directory does this go in" rules.
- **Provider-neutral review and PR commands** — the review discipline without a specific tracker's
  workflow baked in.

## What you'll need to fill in yourself

- The `{{PLACEHOLDER}}` blocks in `CLAUDE.md` — build and test commands, architecture notes,
  conventions, platform gotchas. This is the one file that must be per-project.
- Your own judgment on which discipline directories you'll actually use. Shipping all four and
  using two is fine; the unused READMEs cost nothing.
- `settings.example.json` → `settings.local.json`, with any machine-specific paths.

## Directory map

| Path | What lives there |
|---|---|
| `CLAUDE.md` | Project instructions Claude reads every session. Deliberately short — durable facts and `{{PLACEHOLDER}}`s only, with workflow pushed into `skills/`. |
| `commands/` | Slash commands — explicit "do this now" entry points. Thin by design: each carries frontmatter (`description`, `argument-hint`, `allowed-tools`) and defers detail to a skill or template. |
| `skills/` | The standards, as skills that load on demand when the work is relevant rather than up front. This is where the substance is. |
| `standards/conventions.md` | Naming, lifecycle, and archiving rules — stated **once**, referenced everywhere. |
| `templates/` | The output shapes commands write against (session summary, weekly summary, guide, PR description). |
| `session-summaries/` | One record per significant session. The workhorse. |
| `weekly-summaries/` | Weekly rollups distilled from session summaries — the "what did I ship" view. |
| `plans/` | **Transient.** Plan-mode working artifacts. Gitignored; delete freely once the work lands. |
| `docs/` | **Durable.** Implementation guides and analyses, distinguished by a `type:` field rather than by separate directories. Committed and kept. |
| `examples/` | Worked examples drawn from the public `fact-bank-resume-builder` repo, so every sample record points at real, inspectable work instead of a fictional placeholder. |
| `settings.example.json` | Sanitized settings — copy to `settings.local.json` and fill in local paths. |

The records are what make this work. A command that writes a session summary is only as useful as
the standard defining what a session summary is *for* — the skills carry that, and they're the
real content of this repo.

## Workflow chain

```
/session-summary          → session-summaries/YYYY-MM-DD-<slug>.md
/end-task-session         → session summary + what's next
/generate-weekly-summary  → weekly-summaries/week-of-<date>.md, archives the week
/generate-pr-description  → PR body from the actual diff
/standup-prep             → talking points from recent records
```

Five commands, deliberately. The private version this came from had seventeen — but most were
built for one project's stack, tracker, and task board, and a command that only works in the repo
it was born in doesn't belong in a template. What's here is what survives being dropped into a
codebase it's never seen.

## Quickstart

```bash
# From your project root
git clone https://github.com/JumpManStudios/claude-template .claude
cd .claude
git remote remove origin          # disconnect from the template
```

Then add `.claude/` to your project's `.gitignore` and wire up `CLAUDE.md`. Full instructions,
including the three adoption modes and how to point the clone at your own workspace repo, are in
[SETUP.md](SETUP.md).

## Status

Being assembled in phases from a private version that's been in daily use — genericized and
modernized as it moves over, one focused change per phase.

- [x] **Phase 1** — Scaffold, license, adoption docs
- [ ] **Phase 2** — `CLAUDE.md` skeleton, `standards/conventions.md`, core discipline skills + commands
- [ ] **Phase 3** — PR and review commands
- [ ] **Phase 4** — Provider-neutral issue-tracker commands
- [ ] **Phase 5** — Verification pass and `PACKAGE_CONTENTS.md`

Until Phase 2 lands, the directory map above is the target shape, not shipped files. The checklist
is where it actually stands.

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, reshape it to your own practice.
