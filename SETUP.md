# Local setup

The coding-agent workspace and the coding assistant's configuration are separate things:

- The **workspace** holds canonical guidance, skills, standards, templates, and records. It can be
  named or stored anywhere.
- A **host adapter** uses the assistant's required discovery paths to point back to that workspace.

The instructions below install the workspace at `.agent-workspace/`, keep all agent artifacts out
of the product repository's history, and enable Claude Code for interactive, human-in-the-loop
development. Other locations work, but their adapter paths must be changed explicitly.

This ignored layout is not delivered to cloud agents in a fresh clone. If a project needs that
behavior, first follow this setup to develop and validate the workspace, then use
[docs/cloud-agent-conversion.md](docs/cloud-agent-conversion.md) to choose which non-private
artifacts become part of the product repository.

## Choose how to store the workspace

### Gitignored clone — recommended

Clone this repository into the product project, disconnect it from the public origin, and point it
at a private repository for that project's records.

- Simple ownership and backup.
- Records stay outside the product repository.
- Per-project guidance can evolve independently.

### Git submodule

Use a submodule when multiple projects should consume a pinned shared package and the workspace
does not contain private per-project records. This adds normal submodule clone, update, and CI
requirements.

### Symlink to a central workspace

Useful for one developer across several local projects. It is machine-specific and should not be
the default for a team or CI environment.

## Install a project workspace

Run these commands from the product repository root.

### 1. Clone the package

```bash
git clone https://github.com/JumpManStudios/coding-agent-workspace.git .agent-workspace
cd .agent-workspace
git remote remove origin
```

The public repository name and clone location are not discovery mechanisms. `.agent-workspace` is
the recommended local name because it describes the content without claiming to replace any
host's native configuration directory.

### 2. Add a private workspace remote

Create an empty private repository, then connect it:

```bash
git remote add origin <your-private-workspace-repository-url>
git push -u origin main
cd ..
```

Skip the remote only if local-only, unbacked records are acceptable.

### 3. Keep the product repository clean

Add this to the product repository's `.gitignore`:

```gitignore
# Coding-agent workspace and local discovery adapters
.agent-workspace/
AGENTS.md
CLAUDE.md
.claude/
.agents/
.windsurf/
```

The product repository tracks none of the workspace, instructions, skills, records, or host
configuration. Its code history stays focused on the product. The nested workspace has its own Git
history and remote.

### 4. Customize canonical project guidance

Fill the `{{PLACEHOLDER}}` values in `.agent-workspace/AGENTS.md`: build and test commands,
architecture, conventions, and known traps. This is the canonical instruction body tracked by the
workspace repository; do not create separate rewritten guidance for each assistant.

## Enable Claude Code

Claude Code requires its own discovery paths even though the behavior is shared.

### 1. Project the shared instructions to the product root

On macOS or Linux:

```bash
ln -s .agent-workspace/AGENTS.md AGENTS.md
```

The root file is ignored by the product repository, but editing it follows the link and changes the
canonical file tracked inside `.agent-workspace`.

On Windows, creating symbolic links may require Developer Mode or an elevated shell. If symlinks
are unavailable:

```bash
cp .agent-workspace/AGENTS.md ./AGENTS.md
```

After intentional edits to the copied root file, copy it back before committing the workspace:

```bash
cp ./AGENTS.md .agent-workspace/AGENTS.md
```

### 2. Add Claude's instruction entry point

If the product repository has no root `CLAUDE.md`:

```bash
cp .agent-workspace/adapters/claude/CLAUDE.md ./CLAUDE.md
```

If it already has one, preserve its Claude-specific content and add this import near the top:

```markdown
@AGENTS.md
```

The adapter file imports the projected root `AGENTS.md`. Keep it thin; project guidance belongs in
the canonical workspace file.

### 3. Expose the canonical skill

On macOS or Linux:

```bash
mkdir -p .claude/skills
ln -s ../../.agent-workspace/skills/session-summary .claude/skills/session-summary
```

The link is adapter wiring. The workflow body remains only in
`.agent-workspace/skills/session-summary/SKILL.md`. Claude discovers the linked skill and exposes it
directly as `/session-summary`; a legacy file under `.claude/commands/` is not needed.

If symlinks are unavailable, copy the skill as a fallback:

```bash
mkdir -p .claude/skills
cp -R .agent-workspace/skills/session-summary .claude/skills/
```

A copied skill can drift. Treat `.agent-workspace/skills/` as the source of truth and repeat the
copy after updates.

### 4. Add local Claude settings only if needed

The example is safe to copy: it contains no fake absolute paths, environment variables, tokens, or
attribution override.

```bash
cp .agent-workspace/adapters/claude/settings.example.json .claude/settings.local.json
```

Edit the local file for machine-specific permissions. Keep it untracked. Claude Code works without
this file, so skip it when the defaults are sufficient.

## Use another workspace name or location

The workspace itself may be named anything. Update all adapter projections together:

1. The target of the root `AGENTS.md` link or copy source.
2. The target of each link under `.claude/skills/`.

The root `CLAUDE.md` continues to import `@AGENTS.md`, so it does not change with the workspace
location. The canonical skill resolves its workspace from the real `SKILL.md` location after
symlinks are resolved. If a copied skill cannot resolve the canonical workspace unambiguously, it
must ask for the location rather than guess.

## Verify the Claude installation

First verify the filesystem wiring from the product root:

```bash
test -f .agent-workspace/AGENTS.md
test -e AGENTS.md
test -f .agent-workspace/skills/session-summary/SKILL.md
test -e .claude/skills/session-summary/SKILL.md
git status --short
```

`git status --short` in the product repository should show none of the ignored agent artifacts.
Running it inside `.agent-workspace/` should show intentional guidance or record changes.

Then start a new Claude Code session at the product root and verify:

1. Ask Claude to summarize the active project guidance. It should describe the filled values from
   the root `AGENTS.md` projection.
2. Type `/session-summary` and confirm the skill is available.
3. Invoke it after a small but meaningful test change.
4. Confirm the record was written under `.agent-workspace/session-summaries/`, contains the six
   fixed headings, and reflects the actual product diff.
5. Confirm the product repository sees no agent artifacts, while the workspace repository sees the
   new record.

Do not mark an adapter supported in the compatibility matrix until this runtime verification has
passed in a clean project.

## Migrating from `claude-template`

Existing users who cloned the old repository directly into `.claude/` should preserve that
directory's Git history, records, and local settings while splitting the workspace from Claude's
adapter surface. Follow [docs/migrating-from-claude-template.md](docs/migrating-from-claude-template.md).

## Troubleshooting

**Claude does not see the project guidance.**
Confirm both root files exist, `AGENTS.md` resolves to the workspace, and `CLAUDE.md` imports
`@AGENTS.md`. Start a new session after changing instruction files.

**`/session-summary` is missing.**
Confirm `.claude/skills/session-summary/SKILL.md` resolves. A broken relative symlink is the most
common cause. Restart Claude Code after repairing it.

**The summary was written into the product repository.**
The copied adapter could not identify the canonical workspace. Remove the record, repair the link
or provide the workspace location, and invoke the skill again.

**The product tries to commit `.agent-workspace/`.**
It was tracked before being ignored. Run `git rm -r --cached .agent-workspace/`; this removes it from
the product index without deleting the nested workspace.
