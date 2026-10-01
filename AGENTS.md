# AGENTS.md

## Purpose

This file contains stable rules that coding agents must follow when working in this repository.

Keep this file short and stable.

For additional context consult:

- `docs/product-brief.md`
- `docs/architecture.md`
- `docs/adr/`
- `specs/`
- `STATUS.md`

## Architecture

The backend is a modular monolith organized by domain.

Initial modules:

- `auth`
- `users`
- `projects`
- `tasks`

Each module preserves the internal dependency direction:

```text
controller -> service -> repository
```

Controllers must not contain business logic.
Repositories must not contain business rules.
Services own domain behavior and orchestration.

## Module boundaries

Allowed initial dependencies:

```text
AUTH     -> USERS
PROJECTS -> USERS
TASKS    -> USERS
TASKS    -> PROJECTS
```

Do not introduce reverse dependencies without an explicit architectural review.
A module must not access another module's repository directly.

Allowed:

```text
TaskService
  -> ProjectService
      -> ProjectRepository
```

Not allowed:

```text
TaskService
  -> ProjectRepository
```

## Dependencies

Do not introduce a new external dependency without:

1. explaining why it is necessary;
2. checking whether existing dependencies already provide the capability;
3. obtaining explicit review.

## Data and migrations

Do not assume database schema changes. Any change affecting schema, persisted data, migrations, constraints, or destructive operations must be explicitly identified in the implementation plan.

Do not perform destructive data changes without explicit approval.

## Security

Do not treat frontend restrictions as authorization. Authorization rules must be enforced by the API.

Never commit passwords, tokens, API keys, secrets, or production credentials.

## Scope

Implement only the scope defined by the active spec/task.
Do not add unrelated refactors or functionality without approval.
Before creating a utility or abstraction, check whether equivalent functionality already exists.

## Validation

Before declaring a task complete, execute the repository validation commands.

Commands are placeholders until the stack is initialized:

- Build: `TBD`
- Tests: `TBD`
- Lint: `TBD`
- E2E: `TBD`
- Local start: `TBD`

## Definition of Done

A task is not complete until:

- the acceptance criteria are satisfied;
- relevant tests pass;
- build passes;
- lint passes;
- the implementation has been reviewed;
- no known task-related failure is hidden;
- `STATUS.md` reflects the resulting state;
- remaining risks or limitations are explicitly reported.

## Agent completion report

At the end of a task report:

1. what changed;
2. files affected;
3. validation commands executed;
4. evidence against acceptance criteria;
5. remaining risks or unresolved issues.
