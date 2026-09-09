# Agentic Coding Interview

## Working style

- Inspect before modifying.
- Prefer the smallest change that satisfies the requirements.
- Preserve existing architecture and conventions unless there is a concrete reason not to.
- Surface ambiguous requirements before making business-rule assumptions.
- Do not add dependencies unless the existing stack is insufficient; justify any new dependency first.
- Review generated changes before accepting them.
- Run the relevant tests after each meaningful change.
- Before finishing, run the full test suite and summarize tradeoffs, edge cases, and anything left unresolved.

## First step

When given a task:

1. Read the task and inspect the repository.
2. Do not modify files yet.
3. Summarize:
  - current architecture
  - relevant files
  - existing test coverage
  - likely minimal implementation path
  - ambiguities that need interviewer clarification
4. Then wait for direction or proceed with an approved minimal plan.

## Time-boxing

This is a timed coding exercise. Avoid unnecessary documentation, broad refactors, or environment work unless they directly support the task.