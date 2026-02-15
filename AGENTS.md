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
- Work in small cycles and update repo memory after meaningful progress.
- Escalate to the human only when constraints/requirements are unclear or when scope boundaries change.

### Agent work loop

```mermaid
flowchart TD
    A@{ shape: circle, label: "Start" } --> B[Pick first task from TODO]
    B --> C[Work on task]
    C -->|Finished or cannot handle| E[Update memory + pick next move]
    C -->|Needs clarification| H[Ask Human]
    E -->|Needs clarification| H
    E --> G{Cycles < x?}
    G -->|Yes| B
    G -->|No| I[Human review]
```

Default cycle cap: x = 5.

### Memory
Keep memory in-repo as:
- `memory/DECISIONS.md`
- `memory/LEARNINGS.md`
- `memory/LOG.md`
- `memory/TODO.md`

Update memory when:
- a decision affects future work,
- a learning generalizes beyond the immediate change,
- a work cycle completes or changes direction.
