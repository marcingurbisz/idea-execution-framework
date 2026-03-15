# TODO and LOG workflow options for IEF

## Question

How should IEF handle task tracking and progress logging as the framework grows?

Specific subquestions:

1. should `TODO.md` and `LOG.md` be merged?
2. should IEF support richer task layouts such as one markdown file per todo under a `todo/` directory?
3. can both styles coexist?

## Short answer

- **Do not fully merge** `TODO.md` and `LOG.md`.
- **Do support** an optional richer `todo/` directory.
- **Yes, allow a mixed model**.

## Why not fully merge TODO and LOG?

A single file sounds simpler at first, but it mixes two different views:

- **queue state** — what should happen next
- **execution history** — what already happened

Those views are both important, but they optimize for different reading patterns.

### What `TODO.md` is good at

- fast scanning
- prioritization
- marking in progress / done
- showing the current queue

### What `LOG.md` is good at

- chronological handoff
- explaining why something changed
- showing learning and decisions over time
- supporting restart after interruptions

If both are merged into one file globally, the active queue becomes harder to scan and the history becomes noisier.

## Better recommendation: hybrid, not full merge

Keep:

- `TODO.md` as the queue / dashboard
- `LOG.md` as the chronological handoff log

But allow **task-local logs** for larger items.

That gives the user the readability benefit they want without losing the clean top-level control plane.

## Recommended model

## Level 1 — simple/default mode

Use this for small repos and lightweight work:

- `README.md`
- `TODO.md`
- `LOG.md`
- `memory/`

This stays the default IEF example because it is easy to teach and easy to start with.

## Level 2 — extended task mode

When tasks become larger, allow an optional `todo/` directory:

```text
repo/
  README.md
  TODO.md
  LOG.md
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
- embedded mini-log
- links to commits or related artifacts

## Recommended relationship between root TODO and task files

Use `TODO.md` as the top-level dashboard and pointer list.

Example:

```markdown
## [IN PROGRESS] Refine memory layout
See `todo/doing-now/2026-03-15-refine-memory-layout.md`
```

This keeps the root queue short while allowing richer task detail elsewhere.

## Should logs live inside task files too?

**Yes, optionally.**

Recommended rule:

- keep `LOG.md` for cross-task chronological handoff
- allow task-local progress notes inside `todo/.../*.md` when a task is large or multi-step

That means IEF supports both:

- **global log** for the overall repo story
- **local log** inside the task file for detailed execution notes

## Mixed model recommendation

The best IEF stance is:

- support the current simple mode as the default
- support richer file-per-task mode as an advanced option
- allow mixing the two when useful

Examples:

- small tasks live only in `TODO.md`
- large tasks get a dedicated file under `todo/`
- important repo-wide outcomes still go to `LOG.md`

## Why this fits IEF well

IEF is about durable repo memory, not about one rigid file format.

So the framework should standardize the **roles** of files more than one exact directory shape:

- control plane
- queue
- handoff log
- extended task artifacts
- supporting memory

That is more flexible and closer to real project usage.

## Proposed framework wording

IEF should describe:

- a **default lightweight layout**
- an **optional extended task layout**
- a rule that `TODO.md` stays the top-level queue even when task files exist

## Plain-language conclusion

IEF should **not** replace `TODO.md` with one giant combined TODO+LOG file.

Instead, it should support this progression:

1. simple root `TODO.md` + `LOG.md`
2. optional `todo/` directory with one file per larger task
3. optional task-local logs inside those files
4. keep `LOG.md` as the high-level chronological handoff log

That gives both simplicity and scalability.
