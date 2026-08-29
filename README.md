# Idea Execution Framework

## Vision
IEF is a way of working where a Git repository becomes shared project memory for the human and one or more AI agents.

The goal is to make idea execution efficient, transparent, restartable, and auditable: plans, decisions, artifacts, progress, and learnings are all kept in the repo instead of being scattered across chat history or a context that can be compacted, lost, or available only on one machine.

In practice, the human provides the idea, priorities, constraints, and reviews; the agent helps to turn that into concrete plans, TODO items, implementation steps, repo updates, and commits.

`AGENTS.md` is the operational contract for this way of working. In short, it defines:
- the core principles of IEF
- the human and agent roles
- when and how a main agent introduces persistent roles and delegates bounded workstreams to worker sessions
- the execution loop and TODO/commit rhythm
- the required repo control surfaces: `README.md` plus one discoverable main queue and any linked role/topic ledgers
- when the agent should continue, stop, escalate, and hand off

IEF separates three related concepts:

- a **persistent role** is a repository-defined specialization with a charter, ownership, ledger, history, and handoffs;
- a **worker session** is one temporary model/runtime execution acting for a role or a one-off workstream;
- a **workstream** is the bounded outcome being delivered. A recurring role may execute many workstreams through successive sessions, while a one-off workstream does not require creating a persistent role.

## Documentation (memory) layout

See [AGENTS.md](AGENTS.md) for the definitive description of the repo control plane and documentation (memory) layout.

In short: `README.md` stays at repo root; the queue can live either in `TODO.md` or in topic/role ledgers; each item keeps its own execution log inline in that queue surface; supporting artifacts live under `docs/`. Use `todo-<role_id>.md` for recurring specialists and topic names for one-off workstreams.

## Quick start

Start with a single repo first:

1. Put [AGENTS.md](AGENTS.md) in the repo root.
2. Optionally, add the provided [devcontainer](.devcontainer/devcontainer.json).
3. Start your agent from your normal IDE or CLI.
4. Create `README.md` and a discoverable TODO ledger, or ask the agent to create them and seed them with the idea, constraints, and existing sources.
5. Prompt `Execute the IEF loop for <ledger>`.

### Example prompts

- New project:

  ```text
  Here is the initial idea:
  <idea here>
  Prepare memory according to AGENTS.md, draft the README, and create the initial TODO items.
  ```

- Existing project without README:

  ```text
  Analyze this project, update the README if needed, then create initial TODO items.
  ```

- Execution:

  ```text
  Execute the IEF loop for TODO.md
  ```

### Additional notes

* IEF is designed to work with agentic CLIs and IDE extensions such as Codex, Claude Code, OpenCode, and GitHub Copilot, provided they can load repository instructions and use the tools required by the project.
* The provided [devcontainer](.devcontainer/devcontainer.json) is optional.
* Move to the [Workspace-level setup](#workspace-level-setup) when you specifically want one shared agent environment across multiple repos.
* Current reference setup: Codex CLI with GPT-5.6 Sol (August 27, 2026).

## Typical collaboration rhythm

Prepare or refine the TODO list and prompt "Execute the IEF loop". After the loop is executed, review the results, define the next TODO items + feedback for agent, and start the loop again.

The queue is not limited to tasks written by the human. During execution, the agent adds a focused `NEW` item when it discovers worthwhile, actionable work that is not already represented. Discovery does not silently expand the item currently in progress: the new item records why it matters and returns to normal prioritization.

Finished TODO items do not automatically become good long-term documentation. Part of the ongoing loop is to fold durable outcomes back into `README.md` and/or `docs/` when they should become part of the stable project memory. In practice this is often triggered by the human via follow-up TODO items, but the agent should also do it when it is clearly within scope.

For larger projects with recurring, context-heavy types of work, introduce a small set of persistent roles. The main agent owns decomposition, shared contracts, acceptance and integration; a temporary worker session executes a bounded workstream for one role. The role remains in the repository (`role_id`, charter, owned paths, ledger, history and last handoff), while sessions may be reused or replaced. Start a worker with `Execute the IEF loop on todo/<topic>.md within the delegation contract recorded there`, then let main independently accept and integrate the result.

The current reference setup uses the native subagent mechanism exposed by an agentic CLI or IDE because it offers structured spawning, steering, waiting, and handoff with low startup overhead (for the current Codex example, see [Subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)). This is an implementation choice, not an IEF requirement. Roles may later run as separate CLI processes, CI jobs, services, or another agent backend when they need independent lifetime, automation, or isolation; the repository control plane stays the same. A backend may also support explicit session continuity, such as [`codex exec resume`](https://learn.chatgpt.com/docs/non-interactive-mode#resume-a-non-interactive-session).

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
- Because `AGENTS.md` is at the workspace root (via symlink), compatible agentic CLIs and IDEs can load the same instructions for every project in that workspace.
- Prompt "Execute the IEF loop in projectA repo" to have IEF loop executed on specific project.

## License

MIT — see [LICENSE](LICENSE).
