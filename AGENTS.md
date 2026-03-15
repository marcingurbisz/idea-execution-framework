# Agent Instructions (Shared)

This file is intended to be shared across projects that use IEF.

## Idea Execution Framework (IEF)

### Core principles
- Git repo as shared project state/blackboard.
- Persist plans, decisions, artifacts, and learnings in the repo.
- Repo files are the source of truth; keep them updated as you work.
- If a needed tool, binary, service, or environment capability is unavailable or failing, report that explicitly together with the impact on the task and any workaround attempted.

### Roles
- Human: vision, constraints, approvals for major decisions.
- AI agent: clarifies, proposes plans, executes tasks, keeps repo in sync.

### Iteration rhythm
- Work in cycles and update repo memory after meaningful progress.
- Escalate to the human only when constraints/requirements are unclear or when scope boundaries change.
- Continue to the next highest-priority actionable item in `TODO.md` - do not stop the loop.
- Hard gate between TODO items: after finishing one TODO, do these in order before starting the next TODO: 1) mark it done in `TODO.md`, 2) commit immediately
- Ask the human before continuing only when requirements are ambiguous, risk is high, or scope/priority trade-offs are required.
- When stopping (or handing off), explicitly state the stop condition and why you are stopping now (e.g., blocked and need human input, intentional status checkpoint before continuing, or no actionable work remains) - remember the default is that you continuue with next item from todo list.

### Agent work loop

```mermaid
flowchart TD
    A@{ shape: circle, label: "Start" } --> B[Pick task from TODO]
    B --> C[Work on next task from TODO]
    C -->|Finished| E[Update memory]
    C -->|Needs clarification| H[Ask Human]
    B -->|Needs clarification| H
    E --> Z["Commit"]
    Z --> G{More work?}
    G -->|Yes| B
    G -->|No| I[Human review]
```

Multiple agents may work in the same filesystem at the same time, so:
- Mark the task you picked in `TODO.md` so others know.
- Keep commit granularity aligned to TODO completion (one commit per completed TODO whenever feasible).
- Commit only your own work.

### Repo control plane and memory layout

Keep repo control files at the root as:
- `README.md` - define the current intent, constraints, and how-to-run guidance.
- `LOG.md` - timestamped progress notes and handoffs (what you did); include learnings inline per entry when relevant.
- `TODO.md` - prioritized next actions

Keep supporting memory artifacts under:
- `memory/` - plans, research notes, session artifacts, and other extended memory files

Optional extended task tracking is also allowed under:
- `todo/` - one markdown file per larger task, optionally grouped into subfolders such as `doing-now/`, `doing-today/`, or parked buckets

Rules:
- `TODO.md` remains the top-level queue/dashboard even when `todo/` exists
- `LOG.md` remains the high-level chronological handoff log
- `todo/` is for larger task-local notes, acceptance criteria, or embedded mini-logs when the work benefits from more structure

`LOG.md` template:

```markdown
# Interaction Log

Template:
- Date – [Short description of item].
    - Outcome: [What was done].
    - Learning (optional): [What was discovered].

## Entries
```

Before starting a work cycle in a project, read the full memory contents (at least once per session) so execution is grounded in the current `README.md`, `LOG.md`, `TODO.md`, and relevant supporting artifacts under `memory/`.

When you work on a task, update memory whenever reasonable (for example, after finishing a meaningful part) and at least once per work loop.
