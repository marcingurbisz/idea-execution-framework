# Agent Instructions (Shared)

This file is intended to be shared across projects that use IEF.

## Idea Execution Framework (IEF)

### Core principles
- Git repo as shared project state/blackboard.
- Persist plans, decisions, artifacts, and learnings in the repo.
- Repo files are the source of truth; keep them updated as you work.

### Roles
- Human: vision, constraints, approvals for major decisions.
- AI agent: clarifies, proposes plans, executes tasks, keeps repo in sync.

### Iteration rhythm
- Work in cycles and update repo memory after meaningful progress.
- Escalate to the human only when constraints/requirements are unclear or when scope boundaries change.

### Execution defaults
- Run loops with a cycle cap `x` (default: 5) unless a project sets a different cap.
- Prefer shipping small, complete increments each cycle.
- Use the simplest implementation that satisfies the current scope.

### Agent work loop

```mermaid
flowchart TD
    A@{ shape: circle, label: "Start" } --> B[Pick task from TODO]
    B --> C[Work on task from TODO]
    C -->|Finished| E[Update memory]
    C -->|Needs clarification| H[Ask Human]
    B -->|Needs clarification| H
    E --> Z["Commit"]
    Z --> G{More work?}
    G -->|Yes| B
    G -->|No| I[Human review]
```

Multiple agents may work in the same filesystem at the same time, so:
- Mark the task you picked in `memory/TODO.md` so others know.
- Commit only your own work.

### Memory
Keep memory in-repo as:
- `README.md` - define the current intent, constraints, and how-to-run guidance.
- `memory/LEARNINGS.md` - reusable discoveries ("what surprised us", pitfalls, patterns).
- `memory/LOG.md` - timestamped progress notes and handoffs (what you did, where, what’s next).
- `memory/TODO.md` - prioritized next actions

Before starting a work cycle in a project, read the full memory contents (at least once per session) so execution is grounded in the current `README.md`, learnings, and priorities.

When you work on a task, update memory whenever reasonable (for example, after finishing a meaningful part) and at least once per work loop.
