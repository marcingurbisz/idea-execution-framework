# TODO-local logging workflow for IEF

## Question

How should IEF handle task tracking and progress logging as the framework grows now that the preferred review style is to see logs under the TODO item itself?

Specific subquestions:

1. should logs live under the TODO item by default?
2. should IEF support richer task layouts such as one markdown file per todo/topic under a `todo/` directory?
3. what, if any, role should `LOG.md` still have?

## Short answer

- **Yes**: logs should live under the relevant TODO item by default.
- **Yes**: support an optional richer `todo/` directory.
- **Keep `LOG.md` only as a legacy archive**, not as the default place for new progress notes.

## Decision

The default IEF model should now be:

- `TODO.md` is the top-level queue and dashboard
- each TODO item keeps its own execution log
- a larger task can move into `todo/.../*.md`, and the root TODO item points there
- `memory/` holds supporting research, plans, and session artifacts
- `LOG.md` is optional historical baggage only, for repos that already used it

This matches the human review preference better and keeps the task story in one place.

## Why this is better

### Pros

- easier human review because status and history stay together
- each task keeps its own context, decisions, and outcomes together
- small tasks stay simple in `TODO.md`
- larger tasks can grow into a dedicated task file without changing the overall model
- less duplication between queue state and history

### Cons and trade-offs

- the repo loses one clean global chronological timeline by default
- agents must follow links to `todo/.../*.md` when a task has moved out of the root file
- very long `TODO.md` files can still become noisy if finished tasks are never trimmed or moved

## Recommended structure

## Level 1 — simple/default mode

Use this for small repos and lightweight work:

- `README.md`
- `TODO.md`
- `memory/`

Example inline item:

```markdown
## [DONE 2026-03-15] Refine memory layout
Decided to move progress logging under the task itself.

### Log
- 2026-03-15 - Compared separate `LOG.md` against task-local logging.
- 2026-03-15 - Updated the framework docs to make TODO-local logging the default.
```

## Level 2 — extended task mode

When tasks become larger, allow an optional `todo/` directory:

```text
repo/
  README.md
  TODO.md
  todo/
    doing-now/
      2026-03-15-refine-memory-layout.md
    doing-today/
      2026-03-15-oracle-feedback.md
    near-term-parked/
    mid-term-parked/
  memory/
```

Each task file can contain:

- task description
- acceptance criteria
- notes / findings
- embedded task log
- links to commits or related artifacts

## Recommended relationship between root TODO and task files

Use `TODO.md` as the top-level dashboard and pointer list.

Example:

```markdown
## [IN PROGRESS] Refine memory layout
See `todo/doing-now/2026-03-15-refine-memory-layout.md`
```

This keeps the root queue short while allowing richer task detail elsewhere.

## What `LOG.md` should mean now

If a repo already has a `LOG.md`, keep it only as:

- a historical archive
- a migration aid while moving to TODO-local logs
- an optional index file if a project has a very specific need for one

## Why this fits IEF well

IEF is about durable repo memory, not about one rigid file format.

So the framework should standardize the **roles** of files more than one exact directory shape:

- control plane
- queue
- task-local log
- extended task artifacts
- supporting memory

That is more flexible and closer to real project usage.

## Plain-language conclusion

IEF should default to **logs under the TODO item**.

That item can live either:

1. directly in `TODO.md`
2. in a separate markdown file under `todo/`

That gives the human a much easier review surface without losing the option to scale into richer task files.
