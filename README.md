# Idea Execution Framework

## Vision
IEF is a way of working where the Git repository becomes a shared documentation (memory) between the human and the AI agent or agents.

The goal is to make idea execution efficient, transparent, restartable, and auditable: plans, decisions, artifacts, progress, and learnings are all kept in the repo instead of being scattered across chat history or a context that can be compacted, lost, or available only on one machine.

In practice, the human provides the idea, priorities, constraints, and reviews; the agent helps to turn that into concrete plans, TODO items, implementation steps, repo updates, and commits.

`AGENTS.md` is the operational contract for this way of working. In short, it defines:
- the core principles of IEF
- the human and agent roles
- when and how a main agent introduces persistent roles and delegates bounded workstreams to worker sessions
- the execution loop and TODO/commit rhythm
- the required repo control surfaces: `README.md` plus one discoverable main queue and any linked role/topic ledgers
- when the agent should continue, stop, escalate, and hand off

## Documentation (memory) layout

See [AGENTS.md](AGENTS.md) for the definitive description of the repo control plane and documentation (memory) layout.

In short: `README.md` stays at repo root; the queue can live either in `TODO.md` or in topic/role ledgers; each item keeps its own execution log inline in that queue surface; supporting artifacts live under `docs/`. Use `todo-<role_id>.md` for recurring specialists and topic names for one-off workstreams.

## Quick start

Start with a single repo first:
1. Put [AGENTS.md](AGENTS.md) in the repo root.
2. Put [devcontainer](.devcontainer/devcontainer.json) in repo root (optionally)
3. Start your agent from your normal IDE or CLI.
4. Ask agent to create `README.md`, `TODO.md` seeding it with ideas, constraints, and existing sources (or do it manually if you prefer):
5. Prompt `Execute the IEF loop for TODO.md`.

### Example prompts
  - New project: 
    ```
    Here is the initial idea:
    <idea here>  
    Prepare memory according to AGENTS.md, draft the README, and create the initial TODO items.
    ```
  - Existing project without README:
    ```
    Analyze this project, update the README if needed, then create initial TODO items.
    ```
  - Execution: 
    ```
    Execute the IEF loop for TODO.md
    ```

### Additional notes
* All CLIs and IDE extension (e.g. Codex, Claude Code, GitHub, Opencode, GitHub Copilot) will work fine.
* The provided [devcontainer](.devcontainer/devcontainer.json) is optional.
* I currently use Codex CLI with GPT-5.6 Sol (August 27, 2026)
* Move to the [Workspace-level setup](#Workspace-level-setup) when you specifically want one shared agent environment across multiple repos.

## Typical collaboration rhythm

Prepare or refine the TODO list and prompt "Execute the IEF loop". After the loop is executed, review the results, define the next TODO items + feedback for agent, and start the loop again.

Finished TODO items do not automatically become good long-term documentation. Part of the ongoing loop is to fold durable outcomes back into `README.md` and/or `docs/` when they should become part of the stable project memory. In practice this is often triggered by the human via follow-up TODO items, but the agent should also do it when it is clearly within scope.

For larger projects with recurring, context-heavy types of work, introduce a small set of persistent roles. The main agent owns decomposition, shared contracts, acceptance and integration; a temporary worker session executes a bounded workstream for one role. The role remains in the repository (`role_id`, charter, owned paths, ledger, history and last handoff), while sessions may be replaced. Start a worker with `Execute the IEF loop on todo/<topic>.md within the delegation contract recorded there`, then let main independently accept and integrate the result.

## Workspace-level setup
Keep `idea-execution-framework` as one folder inside a larger multi-project workspace and open the whole workspace as a single VS Code devcontainer.

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

Usage:
- Open `workspace/` in VS Code and/or Agentic CLIs.
- When in VS Code: Reopen in container from the workspace root (the root `.devcontainer` symlink points to this repository's devcontainer config).
- Because `AGENTS.md` is at workspace root (symlink), Agentic CLIs/IDEs includes these instructions in every prompt across all projects in that workspace.
- Prompt "Execute the IEF loop in projectA repo" to have IEF loop executed on specific project.

## License

MIT — see [LICENSE](LICENSE).
