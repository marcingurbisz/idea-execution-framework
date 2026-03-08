# Idea Execution Framework

## Vision
IEF is a lightweight way of working where the Git repository becomes a shared memory between the human and the AI agent.

The goal is to make idea execution efficient, transparent, restartable, and auditable: plans, decisions, artifacts, progress, and learnings are all kept in the repo instead of being scattered across chat history.

In practice, the human provides the idea, priorities, constraints, and reviews; the agent turns that into concrete plans, TODO items, implementation steps, repo updates, and commits.

`AGENTS.md` is the operational contract for this way of working. In short, it defines:
- the core principles of IEF
- the human and agent roles
- the execution loop and TODO/commit rhythm
- the required memory structure: `README.md`, `memory/LOG.md`, and `memory/TODO.md`
- when the agent should continue, stop, escalate, and hand off

### Agent

Start the agent from an IDE extension (e.g. GitHub Copilot) or a CLI (e.g. Codex, Claude Code, GitHub) inside the provided [devcontainer](.devcontainer/devcontainer.json).

Note: I currently use GitHub Copilot in agent mode with GPT-5.4 (March 7, 2026)

## How to use
- Add [AGENTS.md](AGENTS.md) to your workspace or copy to your existing one
- Prompt the agent to prepare repo memory according to the structure defined in [AGENTS.md](AGENTS.md): `README.md`, `memory/LOG.md`, and `memory/TODO.md`
- For a new project, start with the initial idea and ask the agent to prepare the initial README and initial TODO items
- For an existing project without a README, ask the agent to analyze the project and prepare the README plus the memory files
- For an existing project, you can also describe the current idea/goal and ask the agent to generate or refresh the TODO list before execution
- Prompt "Extecute the IEF loop" to start agent work on TODO items

Example prompts:
- New project:
	- "Here is the initial idea: ... Prepare memory according to AGENTS.md, draft the README, and create the initial TODO items."
- Existing project without README:
	- "Analyze this project and prepare README.md plus memory files according to AGENTS.md. Then create initial TODO items."
- Existing project with a new idea:
	- "Based on this idea: ... analyze the current project, update the README if needed, and generate a prioritized TODO list."
- Execution:
	- "Extecute the IEF loop"

## Typical collaboration rhythm

- I prepare or refine the TODO list and prompt "Extecute the IEF loop"
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
- Prompt "Extecute the IEF loop in projectA repo" to have IEF loop executed on specific project.