---
type: guide
title: Migrate an existing claude-template workspace
date: 2026-08-29
status: current
---

# Migrate an existing `claude-template` workspace

This migration preserves the existing workspace repository, its Git history, session records, and
machine-local Claude settings while separating the provider-neutral workspace from Claude's
required `.claude/` adapter directory.

The old layout used `.claude/` for both jobs. The new layout uses:

```text
your-project/
├── .agent-workspace/     # existing workspace repository and records
├── AGENTS.md             # ignored link to canonical workspace guidance
├── .claude/              # thin Claude discovery/settings adapter
└── CLAUDE.md             # ignored file importing root AGENTS.md
```

## Before moving anything

From the existing `.claude/` repository:

```bash
cd .claude
git status
git add -A
git commit -m "Checkpoint workspace before provider-neutral migration"
git push
cd ..
```

Do not continue with uncommitted workspace changes. If the workspace has no remote, create a local
backup before proceeding.

## Bring in the generalized package

The old clone and the renamed public repository share history, so add it as an upstream and merge
the release containing #17:

```bash
cd .claude
git remote add upstream https://github.com/JumpManStudios/coding-agent-workspace.git
git fetch upstream
git merge upstream/main
```

If `upstream` already exists, inspect it with `git remote -v` and correct the URL rather than
adding a duplicate.

The likely conflict is the old project-specific `CLAUDE.md`. Preserve its filled project facts in
the new canonical `AGENTS.md`; keep the new root `CLAUDE.md` thin. Do not copy workflow steps into
either instruction file.

Commit and push the resolved merge before continuing.

## Split workspace and adapter paths

Run from the product repository root:

```bash
mv .claude .agent-workspace
mkdir -p .claude/skills
```

If the old workspace had local Claude settings, move them onto the new adapter surface:

```bash
if [ -f .agent-workspace/settings.local.json ]; then
  mv .agent-workspace/settings.local.json .claude/settings.local.json
fi
```

Project the canonical guidance, install the thin Claude entry point, and link the canonical skill:

```bash
ln -s .agent-workspace/AGENTS.md AGENTS.md
cp .agent-workspace/adapters/claude/CLAUDE.md ./CLAUDE.md
ln -s ../../.agent-workspace/skills/session-summary .claude/skills/session-summary
```

If the product already has a root `CLAUDE.md` with useful Claude-only guidance, keep that guidance
and replace its old `@.claude/CLAUDE.md` import with:

```markdown
@AGENTS.md
```

Remove any legacy `.claude/commands/session-summary.md`. Claude invokes the canonical skill itself
as `/session-summary`, so retaining the wrapper creates a second behavior surface that can drift.

## Update product ignore rules

Replace the old whole-directory ignore:

```gitignore
.claude/
```

with the isolated-workspace ignores:

```gitignore
.agent-workspace/
AGENTS.md
CLAUDE.md
.claude/
.agents/
.windsurf/
```

This keeps all agent guidance, skills, records, and local adapter configuration out of the product
repository. Changes made through the `AGENTS.md` link are tracked by the nested workspace instead.

## Verify before deleting any backup

Follow the runtime verification in [../SETUP.md](../SETUP.md). Confirm all of the following:

- Existing records are present under `.agent-workspace/session-summaries/`.
- The workspace repository still has its expected private remote and history.
- The ignored root `AGENTS.md` resolves to `.agent-workspace/AGENTS.md`.
- Claude reads project guidance through the root projection.
- `/session-summary` resolves to the canonical skill and writes a new record into the workspace.
- `.claude/settings.local.json` remains untracked.

Only remove a temporary backup after those checks pass and the migrated workspace has been pushed.
