# TODO-local logging workflow for IEF

## Question

How should IEF handle task tracking and progress logging as the framework grows now that the preferred review style is to see logs under the TODO item itself?

Specific subquestions:

1. should logs live under the TODO item by default?
2. should IEF support richer task layouts such as one markdown file per todo/topic under a `todo/` directory?
3. whether IEF still needs a separate repo-level log file?

## Short answer

- **Yes**: logs should live under the relevant TODO item by default.
- **Yes**: support an optional richer `todo/` directory.
- **No**: IEF does not need a separate repo-level `LOG.md`.

## Decision

The default IEF model should now be:

- support two queue modes:
  - `TODO.md` mode: one root queue/dashboard
  - `todo/` mode: one markdown file per topic/task as the queue surface
- each item keeps its own execution log in whichever queue surface is used
- in `TODO.md` mode, a larger task can move into `todo/.../*.md`, and the root TODO item points there
- `memory/` holds supporting research, plans, and session artifacts
- there is no separate repo-level `LOG.md`

This matches the human review preference better and keeps the task story in one place.

## Why this is better

### Pros

- easier human review because status and history stay together
- each task keeps its own context, decisions, and outcomes together
- small tasks stay simple in one place
- larger tasks can grow into a dedicated task/topic file without changing the logging model
- less duplication between queue state and history

### Cons and trade-offs

- the repo loses one clean global chronological timeline by default
- agents must follow links to `todo/.../*.md` when a task has moved out of the root file
- very long `TODO.md` files can still become noisy if finished tasks are never trimmed or moved

## Recommended structure

## Level 1 — `TODO.md` mode

Use this for small repos and lightweight work:

- `README.md`
- `TODO.md`
- `memory/`

Example inline item:

```markdown
## [DONE 2026-03-15] Refine memory layout
Decided to move progress logging under the task itself.

Completed changes:
- Compared a separate repo-level log against task-local logging.
- Updated the framework docs to make TODO-local logging the default.
```

## Level 2 — `todo/` mode

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

In this mode, the actionable queue lives directly in topic files, for example:

```text
repo/
  README.md
  todo/
    doing-now/
      2026-03-16-onboarding.md
    doing-today/
      2026-03-16-review-last-commit.md
  memory/
```

## Relationship between root TODO and task files

If the repo uses `TODO.md` mode, use `TODO.md` as the top-level dashboard and pointer list.

Example:

```markdown
## [IN PROGRESS] Refine memory layout
See `todo/doing-now/2026-03-15-refine-memory-layout.md`
```

This keeps the root queue short while allowing richer task detail elsewhere.

If the repo uses `todo/` mode, there is no requirement to keep a root `TODO.md` at all.

Recommended loop semantics in `todo/` mode:

- "Execute the IEF loop for `todo/.../topic.md`" means work only from that topic file
- "Execute the IEF loop for the repo" means process actionable topic files in priority order

## What replaces `LOG.md`

Keep the completed-work narrative under the TODO item itself.

Use `memory/` only for:

- supporting research
- plans and design notes
- session artifacts
- reusable reference material that matters beyond one TODO item

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
