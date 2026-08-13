# Setup

How to adopt this template in a project. Pick one of the three modes below — they trade
simplicity against reuse — then follow the wiring steps.

---

## Choose a consumption mode

### 1. Gitignored clone — *recommended*

Clone the template into your project as `.claude/`, disconnect it from the template's origin, and
point it at your own repository. Each project gets its own independent copy that drifts as that
project's needs drift.

- **Good:** simplest thing that works. No submodule friction, no shared-state surprises. Your
  workspace history is private and separate from the product's.
- **Trade-off:** improvements you make in one project don't propagate to the others. In practice
  this matters less than it sounds — per-project drift is usually *desirable*, and the handful of
  changes worth sharing are easy to cherry-pick.

**This is the right default for nearly everyone.** Start here.

### 2. Git submodule

Add the template as a submodule so every project references a pinned version you can update
deliberately.

- **Good:** genuinely DRY and versioned. One improvement, updated everywhere on your schedule.
- **Trade-off:** submodules are a real cost — detached HEADs, clone/checkout steps teammates
  forget, CI that needs `--recurse-submodules`. Worth it only if you're maintaining many projects
  and actively want them in lockstep.

### 3. Symlink to a central copy

Keep one copy at `~/claude-template` and symlink `.claude/` to it from each project.

- **Good:** maximally DRY, edits are instantly live everywhere.
- **Trade-off:** breaks for anyone but you. Teammates and CI resolve a dangling link, and there's
  no per-project customization at all — one project's tweak silently changes every other.
  **Solo machines only.**

---

## Wiring it up (gitignored clone)

### Step 1 — Clone into your project

From your project root:

```bash
git clone https://github.com/JumpManStudios/claude-template .claude
cd .claude
```

### Step 2 — Disconnect from the template

```bash
git remote remove origin
```

This severs the link to this repo so your workspace history is your own and you never accidentally
push project-specific notes back to the template.

### Step 3 — Point at your own workspace repo (optional but recommended)

Create an empty repository — private, since it will accumulate your working notes — and connect
it:

```bash
git remote add origin <your-workspace-repo-url>
git push -u origin main
```

Skip this only if you're fine with the workspace being local-only and unbacked.

### Step 4 — Have the project ignore `.claude/`

Add to your **project's** `.gitignore` (not the one inside `.claude/`):

```gitignore
# Claude Code workspace — separate repository
.claude/
```

If the project was already tracking it, untrack without deleting:

```bash
git rm -r --cached .claude/
```

### Step 5 — Wire up `CLAUDE.md`

Claude Code reads `CLAUDE.md` from your **project root**, but the version-tracked source of truth
should live inside `.claude/` where it's under the workspace repo's history. Point one at the other
with an import, so there's exactly one file to edit:

```markdown
<!-- CLAUDE.md at project root -->
@.claude/CLAUDE.md
```

Confirm the import resolves in your Claude Code version — start a session and ask Claude what
project instructions it sees. If imports aren't picked up, fall back to copying the file
(`cp .claude/CLAUDE.md ./CLAUDE.md`) and remember that the copy in `.claude/` remains the source of
truth: edit there, copy out, commit in the workspace repo.

Then fill in the `{{PLACEHOLDER}}` markers in `.claude/CLAUDE.md` for this project — build
commands, test commands, architectural conventions, whatever Claude needs to not guess.

---

## Verify

```bash
# The workspace is its own repository, pointed where you expect
cd .claude && git remote -v

# The project does NOT see it
cd .. && git status --porcelain     # .claude/ must not appear
```

Then start a Claude Code session in the project root and confirm your commands are available —
type `/` and look for the ones from `commands/`.

Skills in `skills/` need no wiring: they're picked up from the `.claude/` directory and load
themselves when the work is relevant. If you want to confirm one is visible, ask Claude what
skills it can see.

---

## Troubleshooting

**The project keeps trying to commit `.claude/`.**
It was tracked before you ignored it. `git rm -r --cached .claude/` — this only removes it from the
index, your files stay put.

**Claude doesn't see my project instructions.**
It reads `CLAUDE.md` from the project root. Confirm the file is there and, if you used the import
form, that the path inside it is correct relative to the root.

**My slash commands don't show up.**
They must be `.md` files directly under `.claude/commands/`. Restart the session after adding new
ones.

**I pushed workspace notes to the template by mistake.**
You skipped Step 2. Remove the remote, then force-push the template back to its prior state if you
have write access — or open an issue and it'll get reverted.
