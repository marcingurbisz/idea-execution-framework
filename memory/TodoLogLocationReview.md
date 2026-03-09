# TODO and LOG location review for IEF

## Question

Should `TODO.md` and `LOG.md` stay under `memory/`, or should they move to the repo root beside `README.md`?

## Decision

Move `TODO.md` and `LOG.md` to the **repo root**.

Keep `memory/` for supporting artifacts such as:

- research notes
- plans
- Oracle session artifacts
- future inbox/cards/consolidation files

## Why this is better

### 1. They are control-plane files, not supporting notes

`README.md`, `TODO.md`, and `LOG.md` form the active operating surface of an IEF repo:

- `README.md` = current intent and entry point
- `TODO.md` = next actions and queue state
- `LOG.md` = execution history and handoff trail

These are the three files an agent or human most often needs first.

They should be maximally visible.

### 2. Root-level visibility improves restartability

One of IEF's main goals is that work is easy to restart.

Putting the three core files at the top level makes the repo easier to re-open after time passes:

- less hunting
- better discoverability
- easier prompt phrasing
- clearer “start here” shape for both humans and agents

### 3. `memory/` is becoming more useful as a backing store

IEF now has multiple supporting artifacts already:

- Oracle plans and specs
- research reviews
- Oracle session folders
- future always-on memory structures

That makes `memory/` more valuable as the place for **extended memory artifacts**, not for the main control files.

### 4. Root clutter stays acceptable

The root now contains a small, reasonable operating surface:

- `AGENTS.md`
- `README.md`
- `TODO.md`
- `LOG.md`
- `skills/`
- `memory/`

This is still lightweight and easier to scan than hiding the queue and log one level down.

## Trade-off

The main downside is that the root is slightly less minimal than before.

That trade-off is acceptable because:

- the number of root control files stays very small
- the active working state becomes easier to see immediately
- `memory/` becomes conceptually cleaner

## Updated convention

### Repo root

- `README.md` — intent, constraints, entry point
- `TODO.md` — prioritized next actions
- `LOG.md` — progress log and handoffs
- `AGENTS.md` — operating contract

### Supporting memory

- `memory/` — plans, research, sessions, and other extended artifacts

## Plain-language conclusion

`TODO.md` and `LOG.md` should live beside `README.md`.

That makes the repo's operating state easier to notice, while freeing `memory/` to become the place for richer supporting memory.