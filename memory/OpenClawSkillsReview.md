# OpenClaw skills review for IEF

## Scope

Reviewed OpenClaw's public README, skills documentation, website, and ClawHub pages on 2026-03-08 with one question in mind:

> Which parts of the OpenClaw skills approach are worth adapting to Idea Execution Framework?

Primary references:

- `openclaw/openclaw` README
- https://docs.openclaw.ai/tools/skills
- https://openclaw.ai/
- https://clawhub.ai/

## High-level takeaway

OpenClaw treats skills as **portable, markdown-first capability packs** that teach the agent how to use tools.

That is the part most relevant to IEF.

The interesting idea is not "plugin marketplace" by itself. The interesting idea is this combination:

- skills are plain files and folders
- they can live at workspace scope or shared-user scope
- they are filtered before use based on real capability checks
- they are injected into the agent context in a compact form
- they can be installed, overridden, and hot-reloaded without changing core code

That maps well to IEF's repo-as-memory philosophy.

## How OpenClaw models skills

A skill is a directory containing a `SKILL.md` file with YAML frontmatter plus instructions.

Important properties:

- **AgentSkills-compatible format** — not a proprietary binary plugin model
- **Markdown-first** — the skill itself is readable, reviewable, and editable
- **Tool-teaching** — a skill explains how and when to use a tool, not just that the tool exists
- **Optional user invocation** — a skill can also become a slash command
- **Optional direct tool dispatch** — some commands can bypass the model and call a tool directly

This is a strong fit for IEF because the framework already centers repo files as the source of truth.

## The parts that feel especially strong

### 1. Location hierarchy and override rules

OpenClaw loads skills from multiple places:

1. bundled skills
2. managed/local skills in `~/.openclaw/skills`
3. workspace skills in `<workspace>/skills`
4. optional extra directories

Precedence is explicit: workspace overrides shared, shared overrides bundled.

### Why this matters for IEF

IEF could benefit from the same layered model:

- **repo-local skills** for project-specific workflows
- **user/global skills** for reusable personal patterns
- optional **shared team packs** later

This gives IEF a clean answer to "where should reusable agent knowledge live?" without forcing everything into one global `AGENTS.md`.

## 2. Skills are gated by real capability checks

OpenClaw can filter skills by:

- required binaries on `PATH`
- environment variables
- config flags
- operating system
- install metadata

That means a skill only appears when it is actually usable.

### Why this matters for IEF

This is one of the most transferable ideas.

IEF should avoid telling the agent about skills that are unavailable in the current environment. A lightweight gating layer would reduce hallucinated tool usage and keep prompts smaller.

## 3. Session snapshot plus hot reload

OpenClaw snapshots the eligible skill set when a session starts, but can refresh when skills/config change or a watcher notices updates.

### Why this matters for IEF

IEF could keep skill discovery deterministic per task/session while still allowing controlled refreshes when the repo changes.

That is a good middle ground between:

- full dynamism, which is hard to reason about
- fully static prompts, which get stale quickly

## 4. Security posture is explicit

OpenClaw's docs repeatedly stress:

- third-party skills are untrusted code
- secrets should be injected carefully
- sandboxing matters
- auto-discovery must respect realpath boundaries

### Why this matters for IEF

Skills become a supply-chain surface very quickly. OpenClaw's explicit warnings are a good reminder that a skill system is not just a convenience feature; it is also a trust model.

## 5. ClawHub gives distribution without hiding the files

ClawHub is a searchable, versioned registry for skill folders. That is useful, but the more important design choice is that the unit of distribution is still the skill folder itself.

### Why this matters for IEF

If IEF ever adds a registry, the artifact should still be a repo-friendly folder, not a hidden opaque package format.

## 6. Token impact is treated as a first-class concern

OpenClaw documents the prompt overhead of skills and keeps the injected representation compact.

### Why this matters for IEF

This is a subtle but important design signal: skill systems are not free. If IEF adds skills, the framework should be deliberate about:

- how many skills are shown to the agent
- how verbose each skill is in the active prompt
- whether only eligible or task-relevant skills are surfaced

## What seems most reusable for IEF

### A. Markdown-first skill folders

This is the strongest idea to copy.

Why:

- easy to diff in Git
- easy for humans to audit
- easy for agents to edit
- easy to keep next to project code and memory

### B. Repo-local plus user-global precedence

Recommended IEF shape:

- `skills/` in the repo
- optional `~/.ief/skills` for personal reusable packs
- repo-local wins on name conflicts

That gives IEF local customizability without losing reuse.

### C. Eligibility filtering

Recommended initial gating keys for IEF:

- `requires.bins`
- `requires.env`
- `requires.os`
- maybe `requires.files`

That would already cover most practical cases.

### D. Hot reload on new tasks, not necessarily mid-step

IEF probably does not need OpenClaw's full watcher behavior on day one.

A simpler rule would be enough:

- refresh skills at the start of each new IEF loop item
- optionally refresh when explicitly requested

### E. Explicit trust levels for external skills

A future IEF skills system should probably distinguish:

- built-in / repo-owned skills
- user-owned global skills
- third-party imported skills

That is simpler than a full sandbox system but still preserves the security mindset.

## What I would *not* copy directly into IEF

### 1. Full registry complexity on day one

ClawHub is impressive, but IEF does not need a marketplace first.

Start with local folders and maybe Git-based import/export later.

### 2. Broad per-run secret injection too early

OpenClaw supports per-skill env and API key injection. Useful, but it adds security and debugging complexity.

IEF should likely start with read-only metadata and explicit operator-managed secrets.

### 3. Too much automatic discovery

OpenClaw's larger system benefits from automatic skill search and install. For IEF, silent auto-install would likely be too much too soon.

A better early default:

- suggest a skill
- require explicit install/import
- log the addition in repo memory

### 4. Multi-platform runtime assumptions

OpenClaw spans chat channels, desktop nodes, browser control, voice, and background daemons.

IEF should keep the skill system much smaller and repo-centric unless a project explicitly needs more.

## Practical IEF interpretation

The OpenClaw approach suggests that IEF skills should be treated as **structured reusable instructions**, not as a huge plugin runtime.

A minimal IEF skill could be just:

- a folder in `skills/<name>/`
- a `SKILL.md`
- frontmatter with `name`, `description`, and simple `requires`
- optional examples, guardrails, and references

Example direction:

```md
---
name: github-review
description: Review the current repository state, summarize recent commits, and inspect pull request context.
requires:
  bins: [git]
---

Use this skill when the task requires understanding recent repository changes,
review history, or preparing a concise status summary.

Preferred actions:
- inspect repo status
- inspect recent commits
- inspect current branch and diffs
- summarize findings into repo memory when appropriate
```

## Design ideas IEF could adopt soon

### 1. Add a `skills/` convention to IEF

This would complement `AGENTS.md`, not replace it.

Suggested split:

- `AGENTS.md` = operating contract and global behavior
- `skills/` = reusable focused capability instructions
- `memory/` = task state, plans, logs, and artifacts

### 2. Add simple frontmatter and eligibility rules

Keep the first version intentionally small.

Suggested fields:

- `name`
- `description`
- `requires.bins`
- `requires.env`
- `requires.os`
- maybe `user-invocable`

### 3. Surface only eligible skills to the agent

Even if IEF never builds a full runtime, a small pre-pass that filters skills by environment would improve prompt quality.

### 4. Log skill usage in repo memory

A nice IEF-specific extension would be to record when a skill materially shaped execution, for example in `LOG.md`.

That would make the repo memory more explanatory over time.

### 5. Prefer importable skill folders over a central registry

If IEF later wants sharing, start with:

- copy/import from Git repos
- maybe git subtrees or vendored folders
- explicit review before enabling

That preserves the repo-as-source-of-truth model.

## Best OpenClaw ideas for IEF, in one list

1. **Markdown-first skill folders**
2. **Repo-local plus user-global precedence**
3. **Eligibility gating before prompt injection**
4. **Compact prompt representation for skills**
5. **Explicit trust/security posture**
6. **Hot reload at sensible boundaries**
7. **Versioned, portable skill folders as the unit of sharing**

## Suggested next IEF step

If you want to apply this review concretely, the best next experiment is:

> add a minimal `skills/` convention to IEF, define a tiny `SKILL.md` frontmatter schema, and support repo-local plus optional `~/.ief/skills` loading with simple environment gating.

That captures most of the value without dragging IEF into OpenClaw's full platform scope.
