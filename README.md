# Idea Execution Framework

## Vision
IEF is a way of working where the Git repository becomes a shared memory between the human and the AI agent.

The goal is to make idea execution efficient, transparent, restartable, and auditable: plans, decisions, artifacts, progress, and learnings are all kept in the repo instead of being scattered across chat history and short-lived context.

In practice, the human provides the idea, priorities, constraints, and reviews; the agent turns that into concrete plans, TODO items, implementation steps, repo updates, and commits.

`AGENTS.md` is the operational contract for this way of working. In short, it defines:
- the core principles of IEF
- the human and agent roles
- the execution loop and TODO/commit rhythm
- the required repo control files: `README.md`, `LOG.md`, and `TODO.md`
- optional reusable capability instructions via `skills/`
- when the agent should continue, stop, escalate, and hand off

### Agent

Start the agent from an IDE extension (e.g. GitHub Copilot) or a CLI (e.g. Codex, Claude Code, GitHub). The provided [devcontainer](.devcontainer/devcontainer.json) is optional but recommended when you want a reproducible toolchain, editor setup, and safer automation defaults.

Note: I currently use GitHub Copilot in agent mode with GPT-5.4 (March 7, 2026)

## How to use
1. Put [AGENTS.md](AGENTS.md) in the workspace so the agent always receives the operating instructions.
2. Ask the agent to prepare or refresh the repo control files defined by [AGENTS.md](AGENTS.md): `README.md`, `LOG.md`, and `TODO.md`. Add the idea, constraints, todos, refer to existing sources (if exists).
3. Prompt "Execute the IEF loop" to have the agent work through the TODO items.

### Memory layout

IEF keeps the main control-plane files at repo root:

- `README.md`
- `LOG.md`
- `TODO.md`

Supporting artifacts live under `memory/`.

This is intentional: the root exposes the current operating state immediately, while `memory/` can grow with plans, research, session outputs, and other extended notes without hiding the active queue and handoff trail one level down.

### Optional skills

IEF can also keep reusable capability instructions under `skills/`.

Use this when a workflow should be taught once and reused many times, for example:

- Oracle escalation
- repository review
- release prep
- project-specific operational routines

Current convention:

- `skills/<name>/SKILL.md` is the required entry point
- supporting templates/checklists can live beside it
- `AGENTS.md` stays broad, while `skills/` stays focused and task-shaped

See [skills/README.md](skills/README.md) for the lightweight convention and [skills/oracle-consult/SKILL.md](skills/oracle-consult/SKILL.md) for the first concrete example.

Example prompts:
- New project:
	- "Here is the initial idea: ... Prepare memory according to AGENTS.md, draft the README, and create the initial TODO items."
- Existing project without README:
	- "Analyze this project and prepare README.md plus memory files according to AGENTS.md. Then create initial TODO items."
- Existing project with a new idea:
	- "Based on this idea: ... analyze the current project, update the README if needed, and generate a prioritized TODO list."
- Execution:
	- "Execute the IEF loop"

## Typical collaboration rhythm

- I prepare or refine the TODO list and prompt "Execute the IEF loop"
- After the loop is executed, I review the results and define the next TODO items
- During a normal work day, I usually review the night loop execution in the morning, define new TODOs, and start the loop again
- Then I do the same again in the evening: review results, prepare new TODOs, and restart the loop

## Workspace-level setup

I keep `idea-execution-framework` as one folder inside a larger multi-project workspace and open the whole workspace as a single VS Code devcontainer.

Recommended layout:

```text
workspace/
	.devcontainer -> ./idea-execution-framework/.devcontainer
	AGENTS.md -> ./idea-execution-framework/AGENTS.md
	idea-execution-framework/
	projectA/
	projectB/
	...
```

How this is used:
- Open `workspace/` in VS Code.
- Reopen in container from the workspace root (the root `.devcontainer` symlink points to this repository's devcontainer config).
- Because `AGENTS.md` is at workspace root (symlink), Copilot includes these instructions in every prompt across all projects in that workspace.
- Prompt "Execute the IEF loop in projectA repo" to have IEF loop executed on specific project.