# Development Method

The project is developed incrementally through small, reviewable tasks.

## Source of truth

The repository is the persistent project memory. Codex conversations are temporary.

- `README.md`: project overview and target architecture.
- `AGENTS.md`: instructions every Codex agent must follow.
- `TODO.md`: only the next work to do. Completed items are removed.
- `docs/DECISIONS.md`: important project decisions, recorded in a fixed format.
- `docs/DEVELOPMENT.md`: this development method.
- `docs/tasks/active/<task>.md`: persistent state of an active task.
- `docs/tasks/completed/<task>.md`: archived completed tasks.

There is no memory file per agent. Memory belongs to the project and to tasks.

## Codex workflow

Each Codex agent works on one bounded task.

```text
Define a small task
        ↓
Create docs/tasks/active/<task>.md
        ↓
Start a Codex conversation
        ↓
Codex reads AGENTS.md + relevant project docs
        ↓
Codex creates a dedicated branch + worktree
        ↓
Implement + test
        ↓
Update task/docs if needed
        ↓
Human reviews and understands the diff
        ↓
CI validation
        ↓
Merge into main
        ↓
Move task to completed/ and remove worktree
```

## Rules

- Keep tasks small and independently reviewable.
- Do not work directly on `main`.
- Do not introduce significant architectural changes silently; explain them first.
- Add or update tests whenever behavior changes.
- Record important architectural or technical choices in `DECISIONS.md`.
- Keep `TODO.md` short and current: only upcoming work, never completed history.
- Update the active task file so another Codex session can resume without previous chat context.
- Keep commits small and understandable.

> Git, documentation and tests preserve project knowledge; Codex agents execute isolated, replaceable tasks.
