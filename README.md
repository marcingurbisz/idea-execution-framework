# Idea Execution Framework

## Vision
Make executing ideas with an AI efficient and transparent, with plans, decisions, artifacts, and learnings versioned and human‑auditable. The AI Agent collaborates with the human to clarify the idea and drive execution in observable steps.

Human involvement can change with project phases. For example: 
* high involvment during early idea elaboration
* lower during planning/prototyping
* low during execution within agreed scope

### Agent

Agent is started from the IDE extension (e.g. Github copilot) or CLI (Codex, Claude Code or Github) from [devcontainer](.devcontainer/devcontainer.json)

### Skills

| Skill | Description |
|---|---|
| Browser usage | When a real browser is needed, use Playwright MCP (or an equivalent local automation approach). |
| Execute a task later | Use GitHub Actions to run an agent CLI with a predefined task. |
| Email | TODO |

## How to use
- Agent instructions: [AGENTS.md](AGENTS.md)
- Repo memory: [memory/](memory/) (Decisions, Learnings, Interaction Log, TODO)