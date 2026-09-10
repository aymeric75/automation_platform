# AGENTS.md

Instructions for every Codex agent working on this repository.

## Before coding

Read, in this order:

1. `README.md`
2. `TODO.md`
3. `docs/DECISIONS.md`
4. `docs/DEVELOPMENT.md`
5. the relevant file in `docs/tasks/active/`, if one exists

Do not rely on previous Codex conversation history as project memory.

## Working rules

- Work on one bounded task at a time.
- Do not work directly on `main`.
- Use a dedicated branch and worktree for the task.
- Keep changes minimal and within the task scope.
- Do not introduce significant architectural changes silently; explain them before implementation.
- Add or update tests whenever behavior changes.
- Keep commits small and understandable.

## Project memory

Update the repository when new persistent knowledge is created:

- task-specific progress or open issues → `docs/tasks/active/<task>.md`
- important technical or architectural decisions → `docs/DECISIONS.md`
- development-process changes → `docs/DEVELOPMENT.md`
- next work only → `TODO.md`

`TODO.md` must contain only upcoming work. Remove completed items.

When a task is complete, move its file from:

`docs/tasks/active/`

to:

`docs/tasks/completed/`

The repository, documentation and tests are the source of truth.
