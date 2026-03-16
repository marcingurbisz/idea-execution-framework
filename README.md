# Idea Execution Framework

## Vision
IEF is a way of working where the Git repository becomes a shared memory between the human and the AI agent.

The goal is to make idea execution efficient, transparent, restartable, and auditable: plans, decisions, artifacts, progress, and learnings are all kept in the repo instead of being scattered across chat history and short-lived context.

In practice, the human provides the idea, priorities, constraints, and reviews; the agent turns that into concrete plans, TODO items, implementation steps, repo updates, and commits.

`AGENTS.md` is the operational contract for this way of working. In short, it defines:
- the core principles of IEF
- the human and agent roles
- the execution loop and TODO/commit rhythm
- the required repo control files: `README.md` plus either `TODO.md` or `todo/` topic files
- optional reusable capability instructions via `skills/`
- when the agent should continue, stop, escalate, and hand off

## Memory layout

See [AGENTS.md](AGENTS.md) for the authoritative description of the repo control plane and memory layout.

In short: `README.md` stays at repo root; the queue can live either in `TODO.md` or in `todo/<topic>.md` files; each item keeps its own execution log inline in that queue surface; supporting artifacts live under `memory/`.

## Quick start

Start with a single repo first:
1. Put [AGENTS.md](AGENTS.md) in the repo root.
2. Put [devcontainer](.devcontainer/devcontainer.json) in repo root (optionally)
3. Start your agent from your normal IDE or CLI.
4. Ask agent to create `README.md`, `TODO.md` (or `todo/<topic>.md` files if you prefer topic-based execution) seeding it with ideas, constraints, and existing sources (or do it manually if you prefer):
5. Prompt `Execute the IEF loop` or `Execute the IEF loop for todo/<topic>.md`.

### Additional notes
* All IDE extension (e.g. GitHub Copilot) or a CLI (e.g. Codex, Claude Code, GitHub) should work fine. 
* The provided [devcontainer](.devcontainer/devcontainer.json) is optional but recommended when you want a reproducible toolchain, editor setup, and safer automation defaults when working with agents from VS Code.
* I currently use GitHub Copilot in VS Code in agent mode with GPT-5.4 (March 7, 2026)
* Move to the [Workspace-leve setup](#Workspace-level-setup) when you specifically want one shared agent environment across multiple repos.
* Example prompts:
  - New project:
      - "Here is the initial idea: ... Prepare memory according to AGENTS.md, draft the README, and create the initial TODO items."
  - Existing project without README:
      - "Analyze this project and prepare README.md plus memory files according to AGENTS.md. Then create initial TODO items."
  - Existing project with a new idea:
      - "Based on this idea: ... analyze the current project, update the README if needed, and generate a prioritized TODO list."
  - Execution:
      - "Execute the IEF loop"
      - "Execute the IEF loop for todo/doing-now/my-topic.md"
      - "Execute the IEF loop across all topic files"

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

## License

MIT — see [LICENSE](LICENSE).