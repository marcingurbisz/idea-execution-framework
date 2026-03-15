# IEF skills

`skills/` is the place for reusable capability instructions that sit between the broad operating contract in `AGENTS.md` and the repo-specific operating state captured in `README.md`, `LOG.md`, `TODO.md`, and supporting artifacts under `memory/`.

## Purpose

Use a skill when you want a focused, repeatable instruction pack for a specific kind of work.

## Relationship to other repo files

- `AGENTS.md` — broad agent operating contract
- `README.md` — project intent and entry points
- `LOG.md` / `TODO.md` — active execution state
- `memory/` — research, plans, session artifacts, and other extended memory files
- `skills/` — reusable capability instructions

## Lightweight convention

The first IEF convention is intentionally simple:

- one folder per skill: `skills/<skill-name>/`
- required file: `SKILL.md`
- optional supporting files: templates, examples, checklists, references

Suggested frontmatter fields for `SKILL.md`:

```yaml
---
name: example-skill
description: Short description of what the skill teaches the agent to do.
requires:
  bins: [git]
  env: []
  os: []
---
```

Notes:

- keep skills human-readable and Git-friendly
- prefer markdown over opaque config formats
- keep instructions specific enough to be reusable, but not so narrow that they only fit one task
- if a skill materially shapes the execution of a task, record that in `LOG.md`