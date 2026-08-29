---
type: guide
title: Converting a project for cloud agents
date: 2026-08-29
status: current
---

# Converting a project for cloud agents

The default workspace model is deliberately local and human-in-the-loop. Its nested repository,
root instruction projections, and host adapter directories are ignored by product Git. A cloud
agent receiving only a fresh product clone cannot see them.

The provider-neutral core is still reusable in a cloud-capable project. The difference is delivery:
the project must track the safe context the agent needs or install it before the agent starts.

## Decide whether to convert

Use the isolated local model when one developer owns the agent workflow, private records should
remain separate, and interactive review is the normal operating style.

Use a project-integrated model when teammates, CI, or cloud agents must receive the same guidance
from a clean clone. This makes agent-facing instructions part of the product's reviewed interface,
with the same maintenance expectations as other project configuration.

## Classify the workspace content

| Content | Project-integrated treatment |
|---|---|
| Project guidance in `AGENTS.md` | Review for secrets and commit at the product root. |
| Canonical reusable skills | Commit under each verified host's documented discovery path, or install them during environment setup. |
| Thin host instruction adapters | Commit at the host's documented project path. |
| Standards and templates required by skills | Commit or install with the skills so references resolve in a clean clone. |
| Session and weekly summaries | Keep private by default; publish only records intentionally treated as project documentation. |
| Plans and scratch output | Keep transient and ignored. |
| Tokens, credentials, permissions, and machine-local settings | Never promote; configure through the cloud platform's secret and environment controls. |

## Conversion outline

1. Start from a working local installation and identify the smallest instruction, skill, standard,
   and template set needed by the intended cloud workflow.
2. Remove personal information, private records, absolute paths, credentials, and assumptions about
   locally installed tools.
3. Place or install those artifacts at the coding agent's documented discovery paths. Do not assume
   that support for `AGENTS.md` implies support for every skill or settings path.
4. Update skill output paths. A cloud run cannot write durable records into an absent ignored
   workspace; write an intentional project artifact, upload an external artifact, or disable that
   output.
5. Review the promoted files like product configuration. Changes now affect every developer and
   automated agent that consumes the repository.
6. Test from a clean clone in the actual cloud-agent environment, including instruction discovery,
   skill discovery, referenced files, tool availability, output retention, and secret handling.
7. Record support only for the host and execution surface that passed. Local verification does not
   establish cloud support, and one host's cloud verification does not establish another's.

## What remains intentionally separate

Conversion does not require moving the entire private workspace into product Git. A developer can
keep `.agent-workspace/` for private summaries and local experiments while the product repository
tracks a smaller, reviewed set of team and cloud instructions. The two sets then have different
owners: shared project behavior changes through product pull requests, while personal workflow
records remain in the nested workspace.

Avoid bidirectional copying between those sets. If a local experiment becomes shared behavior,
promote it deliberately through review. Once promoted, maintain the project-integrated copy as the
team-facing source rather than silently syncing private edits into it.

## Support boundary

This guide describes a conversion strategy, not a verified cloud adapter. Cloud platforms differ
in checkout behavior, initialization hooks, persistence, secret injection, and supported agent
surfaces. Claim support only after a clean-room test in the named platform and document the exact
installation used.
