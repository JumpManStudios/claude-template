---
type: analysis
title: Isolated workspace and adapter projection model
date: 2026-08-29
status: current
---

# Local workspace and adapter projection model

## Decision

For local, human-in-the-loop development, install `coding-agent-workspace` as a nested Git
repository ignored by the product repository. Keep canonical instructions, skills, standards,
templates, and generated records inside that nested repository. Project the files coding
assistants must discover into the product root through ignored relative symlinks.

The recommended local name is `.agent-workspace/`, but the name is configurable because every host
adapter points to it explicitly.

```text
product-repository/              # product Git ownership
├── .agent-workspace/            # separate workspace Git ownership; ignored by product
│   ├── AGENTS.md                # canonical project-specific instructions
│   ├── skills/                  # canonical workflow bodies
│   ├── standards/
│   ├── templates/
│   └── session-summaries/       # private durable records
├── AGENTS.md                    # ignored link to .agent-workspace/AGENTS.md
├── CLAUDE.md                    # ignored thin Claude entry point
└── .claude/skills/              # ignored links to canonical skills
```

Future local adapters follow the same pattern with their documented discovery paths.

## Why this model

The product repository remains about the product. Agent skills, private working records, local
permissions, and evolving project guidance do not appear in its status, commits, pull requests, or
public history.

The workspace still has full version control. Editing the projected root `AGENTS.md` follows the
link into `.agent-workspace/AGENTS.md`, so the improvement appears in the workspace repository
without a manual synchronization step.

The layout also separates names from discovery. `.agent-workspace/` is an ownership boundary, not
a path any coding assistant is expected to discover automatically. Root and host-specific adapter
paths satisfy discovery requirements.

## Adapter mechanism

Relative symlinks are the primary mechanism:

- They preserve one source of truth.
- They work after the product and nested workspace move together.
- They make edits through the projected path visible to workspace Git immediately.
- They require no generator or drift-check script.

Copying is the fallback where symlinks are unavailable. Copied instructions must be copied back
after intentional edits, and copied skills must be refreshed after workspace updates. The copy is
never canonical.

## Alternatives rejected

### Use `.claude/` as the workspace root

Rejected because the directory is a Claude Code discovery and configuration surface. Other hosts
do not treat `.claude/AGENTS.md` or `.claude/skills/` as their canonical project locations.

### Commit selected agent artifacts to the product repository

Not selected for the local model because it changes the ownership goal: shared instructions and
skills become project changes. It is nevertheless the appropriate starting point when teammates,
CI, or cloud agents must receive the workspace from a fresh clone. Private records, credentials,
and machine-local settings still should not be committed. The conversion is documented in
[Converting a project for cloud agents](cloud-agent-conversion.md).

### Generate adapter files

Rejected for the current package because generation adds tooling, regeneration rules, and CI drift
checks without improving the one-workspace local use case.

### Maintain separate host workflow copies

Rejected because behavioral fixes would need to be repeated and would eventually diverge. Host
adapters expose canonical skills; they do not rewrite them.

## Consequence for cloud agents

Ignored files are absent from a fresh product clone. This model therefore supports local coding
assistants first and deliberately favors close human review during execution. Cloud agents require
either committed adapter artifacts or environment setup that installs the workspace before the
agent starts. A local verification result must not be used to claim cloud support.

This is a scope choice, not a claim that cloud agents are inferior or incompatible with the core.
The portable content can be promoted into a project-integrated layout when that tradeoff is useful;
the resulting cloud workflow must be tested independently.
