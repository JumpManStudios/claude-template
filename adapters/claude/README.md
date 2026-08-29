# Claude Code adapter

Claude Code reads `CLAUDE.md`, discovers project skills under `.claude/skills/`, and reads project
settings from `.claude/settings.json` or `.claude/settings.local.json`. Those paths are Claude's
adapter surface; the canonical instructions and workflow bodies remain in the provider-neutral
workspace.

## Recommended project wiring

Assuming the workspace is cloned at `.agent-workspace/` and ignored by the product repository:

1. Project `.agent-workspace/AGENTS.md` into the product root:

   ```bash
   ln -s .agent-workspace/AGENTS.md AGENTS.md
   ```

2. Copy `adapters/claude/CLAUDE.md` to the project root as the thin, generated `CLAUDE.md`.
3. Create `.claude/skills/` in the project.
4. Link each canonical skill into that directory. For `session-summary` on macOS or Linux:

   ```bash
   mkdir -p .claude/skills
   ln -s ../../.agent-workspace/skills/session-summary .claude/skills/session-summary
   ```

5. Optionally copy `adapters/claude/settings.example.json` to
   `.claude/settings.local.json` and customize it.

Claude exposes discovered skills as slash commands, so the canonical `session-summary` skill is
invoked as `/session-summary`; no legacy command wrapper is required.

If the workspace has another name or location, change the root `AGENTS.md` link and skill-link
targets together. On systems where symlinks are unavailable, copy the instruction and skill files;
copy intentional instruction edits back into the workspace and refresh the projections after
workspace updates. This is a compatibility fallback, not a second source of truth.

## Verify

Start Claude Code at the adopting project's root and confirm:

- It can summarize the active project guidance from the root `AGENTS.md` projection.
- `/session-summary` appears as an available skill.
- Invoking it writes to `.agent-workspace/session-summaries/`, not the product repository.
- `.claude/settings.local.json`, when used, remains untracked.

This ignored adapter model targets local, human-in-the-loop coding sessions. A cloud agent receives
only files present in the product clone, so cloud use requires committed adapter files or
environment setup that installs the workspace before the agent starts. The
[conversion guide](../../docs/cloud-agent-conversion.md) describes that boundary; cloud support
must be verified and claimed separately.
