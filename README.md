# Idea Execution Framework

## Vision
Make executing ideas with an AI efficient and transparent, with plans, decisions, artifacts, and learnings versioned and human‑auditable. The AI Agent collaborates with the human to clarify the idea and drive execution in observable steps.

Human involvement can change with project phases. For example: 
* high involvement during early idea elaboration
* lower during planning/prototyping
* low during execution within agreed scope

### Agent

Start the agent from an IDE extension (e.g. GitHub Copilot) or a CLI (e.g. Codex, Claude Code, GitHub) inside the provided [devcontainer](.devcontainer/devcontainer.json).

## How to use
- Add [AGENTS.md](AGENTS.md) to your workspace or copy to your existing one
- Prompt the agent to prepare repo memory according to the structure defined in [AGENTS.md](AGENTS.md): `README.md`, `memory/LOG.md`, and `memory/TODO.md`
- For a new project, start with the initial idea and ask the agent to prepare the initial README and initial TODO items
- For an existing project without a README, ask the agent to analyze the project and prepare the README plus the memory files
- For an existing project, you can also describe the current idea/goal and ask the agent to generate or refresh the TODO list before execution
- Prompt "Extecute the IEF loop" or similar

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

## Workspace-level setup (how I use it)

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
- Run tasks from `workspace/` root.
- Because `AGENTS.md` is at workspace root (symlink), Copilot includes these instructions in every prompt across all projects in that workspace.