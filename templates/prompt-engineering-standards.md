# Prompt Engineering Standards

## New Feature Prompts

### Poor Prompt
```
Build a task API
```

### Good Prompt
```
Stack: Node + Express + TypeScript.
Add POST /tasks: { title: string (1-120), dueDate?: ISO8601 }.
Return 201 { id, title, dueDate? }. 400 on validation errors.
Files: src/routes/tasks.ts only. No new dependencies.
```

### Improved Prompt
```
Context: existing app in src/app.ts, in-memory store pattern in src/store.ts.
Task: POST /tasks — validate title 1-120 chars, optional ISO8601 dueDate.
Errors: 400 { error: string }. Success: 201 { id: uuid, title, dueDate? }.
Also generate vitest tests: happy path, empty title, invalid date.
Do not add packages beyond what is in package.json.
```

## Debugging Prompts

### Poor Prompt
```
fix the bug
```

### Good Prompt
```
POST /tasks returns 500 when dueDate is omitted (should be optional).
Stack trace: TypeError at validateDueDate (tasks.ts:42).
Expected: 201 with dueDate null/omitted. Actual: 500.
Show minimal fix in validation layer only.
```

### Improved Prompt
```
Bug: POST /tasks 500 when body is { "title": "Ship feature" } (no dueDate).
File: src/validation/taskSchema.ts — validateDueDate treats undefined as invalid.
Constraint: keep optional dueDate; add regression test in tasks.test.ts.
Do not change route handler except if required for error mapping.
```

## Code Review Prompts

### Poor Prompt
```
make this code better
```

### Good Prompt
```
Review src/middleware/auth.ts for:
1) missing rate limiting on /login
2) JWT expiry validation
3) error response shape consistency
List issues by severity; suggest patches inline.
```

### Improved Prompt
```
Security review — auth middleware only (src/middleware/auth.ts).
Threat model: public internet, brute-force login, token replay.
Output: table [severity, line, issue, suggested fix].
Do not refactor unrelated files. Flag any new dependency proposals.
```

---
> **Pro Tip**  - Four focused prompts in ten minutes usually beat one mega-prompt that hallucinates dependencies. Interviewers watch your iteration strategy.