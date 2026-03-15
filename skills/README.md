# IEF skills

`skills/` is the place for reusable capability instructions that sit between the broad operating contract in `AGENTS.md` and the repo-specific operating state.

## Purpose

Use a skill when you want a focused, repeatable instruction pack for a specific kind of work.

## Relationship to other repo files

For the authoritative description of control-plane files and memory layout, see [AGENTS.md](../AGENTS.md).

In short:

- `AGENTS.md` defines the broad contract
- `skills/` adds reusable focused capability instructions
- the active queue, task logs, and supporting memory stay in the repo structures described in `AGENTS.md`

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
- if a skill materially shapes the execution of a task, record that under the relevant TODO item or its linked task file