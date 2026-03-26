# Research: standards for reusable agent skill and instruction packs

Date: 2026-03-15

## Question

Is there a real standard for how IDE/CLI AI agents load reusable skill or instruction packs, and what should IEF align with?

## Short answer

There is **no universal standard** that all prominent coding agents share.

There are, however, **two real emerging conventions** with clear evidence:

1. **`AGENTS.md`** for persistent repo/worktree instructions.
2. **Agent Skills / `SKILL.md`** for reusable capability packs.

These are **not the same thing**:
- `AGENTS.md` is a human-authored instruction file for an agent working in a repo or directory tree.
- `SKILL.md` is the entrypoint for a reusable capability package with optional scripts, references, and assets.

The closest thing to a cross-vendor skills standard is now **Agent Skills**. The closest thing to a cross-vendor repo-instructions convention is **AGENTS.md**.

## Key findings

## 1. No single universal standard spans all major coding agents

The main vendors do not all use the same mechanism:
- **OpenAI Codex** uses both `AGENTS.md` and Agent Skills / `SKILL.md`.
- **Claude Code** uses both `CLAUDE.md`/`.claude/rules/` and Agent Skills / `SKILL.md`.
- **OpenClaw** uses AgentSkills-compatible `SKILL.md` folders.
- **GitHub Copilot** emphasizes `.github/copilot-instructions.md`, path-specific `.instructions.md`, `AGENTS.md`, and custom `.agent.md` profiles rather than a documented `SKILL.md` pack system.

So the ecosystem is converging around **two partially shared layers**, not one:
- a **repo instruction layer** (`AGENTS.md`, `CLAUDE.md`, Copilot instruction files)
- a **reusable skills layer** (`SKILL.md` via Agent Skills)

## 2. `AGENTS.md` is a real open convention, but for instructions, not skills

The AGENTS.md project describes itself as “a simple, open format for guiding coding agents,” positions it as “a README for agents,” and says it is used by over 60k open-source projects.

Important properties:
- plain Markdown, no required fields
- hierarchical / nested precedence
- intended for build steps, testing guidance, conventions, and workflow rules
- not a reusable capability-pack format

This is best understood as a **persistent instruction contract**, not a skill-pack spec.

Sources:
- AGENTS.md home page: https://agents.md/
- AGENTS.md repo: https://github.com/agentsmd/agents.md

## 3. Agent Skills / `SKILL.md` is the closest real cross-agent skill-pack standard

The Agent Skills site describes the format as “a simple, open format for giving agents new capabilities and expertise.” Its specification defines:
- one skill per directory
- required `SKILL.md`
- required frontmatter fields: `name`, `description`
- optional `scripts/`, `references/`, `assets/`
- progressive disclosure: metadata first, full skill content only when activated

The site also claims open development and growing adoption, and says the format was originally developed by Anthropic and released as an open standard.

Sources:
- Agent Skills overview: https://agentskills.io/
- Agent Skills spec: https://agentskills.io/specification

## 4. OpenAI Codex explicitly supports both layers

OpenAI’s official Codex docs say:
- `AGENTS.md` is used for layered persistent instructions, discovered from `~/.codex/AGENTS.md` plus project/root-to-CWD files, with optional `AGENTS.override.md` and fallback filenames.
- Agent Skills are a separate feature that “build on the open agent skills standard.”
- Skills are directories with `SKILL.md` plus optional scripts/resources.
- Codex scans `.agents/skills` from CWD up to repo root, plus user/admin/system locations.

This is strong evidence that OpenAI treats **`AGENTS.md` and `SKILL.md` as distinct, complementary mechanisms**.

Sources:
- Codex AGENTS guide: https://developers.openai.com/codex/guides/agents-md
- Codex skills guide: https://developers.openai.com/codex/skills

## 5. Claude Code explicitly supports both `CLAUDE.md` and Agent Skills

Anthropic’s docs make the separation very clear:
- `CLAUDE.md` gives persistent instructions for a project, user, or organization.
- `.claude/rules/` provides modular path-scoped rules.
- Skills are the on-demand capability mechanism, and Anthropic explicitly says Claude Code skills follow the **Agent Skills open standard**, while also extending it with extra features.

Claude Code skill locations are:
- `~/.claude/skills/<skill-name>/SKILL.md`
- `.claude/skills/<skill-name>/SKILL.md`
- plugin `skills/` directories

This is the clearest evidence that **repo instructions and reusable skills are separate product concepts**.

Sources:
- Claude Code memory / `CLAUDE.md`: https://code.claude.com/docs/en/memory
- Claude Code skills: https://code.claude.com/docs/en/skills

## 6. OpenClaw explicitly claims AgentSkills compatibility and defines concrete loading rules

OpenClaw’s skills docs say:
- “OpenClaw uses AgentSkills-compatible skill folders to teach the agent how to use tools.”
- A skill is a directory containing `SKILL.md` with YAML frontmatter and instructions.
- Skills load from three main places:
  1. bundled skills
  2. managed/local skills in `~/.openclaw/skills`
  3. workspace skills in `<workspace>/skills`
- Precedence is: `<workspace>/skills` > `~/.openclaw/skills` > bundled skills
- Extra skill directories can be configured via `skills.load.extraDirs` at lowest precedence.
- Skills can also be provided by plugins and installed from ClawHub.
- OpenClaw applies load-time eligibility filters via `metadata.openclaw` for OS, required binaries, env vars, and config.
- OpenClaw snapshots eligible skills per session and can hot-refresh them with the skills watcher.
- OpenClaw’s parser is stricter than the generic spec in some areas: single-line frontmatter keys only, and `metadata` should be a single-line JSON object.

This is not just a vague compatibility claim; OpenClaw documents a real loading contract.

Source:
- OpenClaw skills docs: https://docs.openclaw.ai/tools/skills

## 7. GitHub Copilot does not present `SKILL.md` as its main reusable-pack mechanism

GitHub Copilot’s official docs emphasize:
- repository-wide instructions in `.github/copilot-instructions.md`
- path-specific instructions in `.github/instructions/**/*.instructions.md`
- agent instruction files via `AGENTS.md`
- root-level compatibility with `CLAUDE.md` and `GEMINI.md`
- custom agents defined in `.github/agents/*.agent.md`

This is evidence that Copilot is aligned much more strongly with **instruction files and custom agent profiles** than with a documented `SKILL.md`-style capability-pack system.

Sources:
- Copilot CLI custom instructions: https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
- Copilot custom agents: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents

## What this means for IEF

IEF should align with **both** emerging conventions, but keep them separate:

### A. Keep `AGENTS.md` as the canonical IEF repo/worktree instruction contract

Why:
- it already matches IEF’s current operating model
- it is cross-tool enough to be useful today
- it is simple Markdown and stable to author by hand
- OpenAI Codex and GitHub Copilot explicitly support it, and the AGENTS.md project is positioning it as an open cross-agent convention

### B. If IEF defines reusable capabilities, make them Agent Skills-compatible `SKILL.md` folders

Why:
- this is the closest thing to a real multi-vendor skills standard
- OpenAI Codex, Claude Code, and OpenClaw all now document support for Agent Skills / `SKILL.md`
- it gives IEF a portable capability-pack format instead of inventing a private one

### C. Keep the portable subset minimal

For IEF’s core skill contract, prefer the portable subset:
- directory-per-skill
- required `SKILL.md`
- required `name` and `description`
- optional `scripts/`, `references/`, `assets/`
- descriptions written so agents can decide when to load the skill

Do **not** make vendor-specific extensions part of IEF’s required core contract.
If IEF needs extensions, put them in namespaced metadata and treat them as optional adapters.

### D. Treat vendor-specific instruction files as compatibility adapters, not the canonical model

Useful files to support when needed:
- `CLAUDE.md`
- `.github/copilot-instructions.md`
- `.github/instructions/**/*.instructions.md`
- `.github/agents/*.agent.md`

But these should be treated as **tool-specific integration surfaces**, not as the main IEF abstraction.

## Recommended IEF position

Recommended wording:

- **Repo instructions:** IEF standardizes on `AGENTS.md` for persistent repo-level and nested working instructions.
- **Reusable capabilities:** IEF standardizes on Agent Skills-compatible `skills/<skill-name>/SKILL.md` folders for reusable skill packs.
- **Tool adapters:** `CLAUDE.md`, Copilot instruction files, and custom agent profiles are optional compatibility layers for specific vendors.

This gives IEF a contract that is:
- portable
- evidence-backed
- already compatible with multiple real tools
- less likely to age badly than a custom format

## Important uncertainties and limits

- Agent Skills adoption claims partly come from the Agent Skills project itself, so the broad ecosystem claim should be treated as directional rather than definitive.
- However, the most important adoption evidence is independently confirmed by official vendor docs from OpenAI, Anthropic, and OpenClaw.
- GitHub Copilot documentation currently supports instruction files and agent profiles, but does not document `SKILL.md` as a first-class cross-project skills feature. That means the `SKILL.md` story is still not universal across top coding agents.
- These products are evolving quickly, so precedence rules, file locations, and compatibility aliases may continue to change.
