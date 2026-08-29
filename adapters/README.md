# Host adapters

The shared core defines behavior once. An adapter only connects that behavior to a coding
assistant's native discovery, configuration, permission, and invocation mechanisms.

## Contract

An adapter may contain:

- An always-on instruction entry point that imports or points to `AGENTS.md`.
- Symlinks that expose canonical instructions or `skills/` through the host's discovery path.
- Host-specific settings, permissions, hooks, workflows, and tool names.
- Verification steps and documented capability differences.

An adapter must not contain:

- A second copy of a canonical workflow body.
- Host-specific metadata inside a portable `SKILL.md`.
- A claim of support without a recorded clean-room test.

## Lifecycle

Adapters move through three states in the compatibility matrix:

1. **Planned** — tracked, but not implemented.
2. **Implemented; verification pending** — files exist, but the clean-room test has not passed.
3. **Supported and verified** — the documented installation and representative workflow passed
   in a clean project.

Recognizing `AGENTS.md` or the Agent Skills format is compatibility evidence, not by itself a
supported adapter.

## Installation mechanism

The primary mechanism is a relative symlink from the product repository's ignored discovery path
to the canonical file inside the nested workspace repository. This preserves one editable source:
editing the projected file changes the tracked file in the workspace repository.

Copying is the compatibility fallback for systems where symlinks are unavailable. A copied file is
not another source of truth; it must be copied back to the workspace after intentional edits and
refreshed from the workspace after package updates. Generated adapters are not part of the current
contract.
