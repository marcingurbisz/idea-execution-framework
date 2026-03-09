# Thin `ief-oracle` wrapper spec

## Purpose

Define the smallest wrapper IEF would need in order to turn Oracle from a manual convention into a repeatable repo-local workflow.

This is intentionally a **thin wrapper over Oracle**, not a reimplementation of Oracle.

## Primary goal

One command should:

1. create a repo-local session folder
2. write the request artifact before execution
3. invoke Oracle in the chosen mode
4. store the final response and metadata in the same session folder

## Command surface

### `ief-oracle ask`

Main entry point.

Example shape:

```text
ief-oracle ask \
  --prompt "Review this architecture decision" \
  --file "src/**/*.ts" \
  --mode browser \
  --model gpt-5.4 \
  --session-id architecture-review-2026-03-09
```

Suggested options:

- `--prompt <text>` — inline prompt
- `--prompt-file <path>` — prompt body from file
- `--file <glob-or-path>` — repeatable input files
- `--mode <browser|api|auto>` — default `auto`
- `--model <name>` — Oracle/OpenAI model label
- `--session-id <slug>` — explicit session folder name
- `--notes <text>` — optional local notes for `meta.json`
- `--wait` — wait/block when API mode detaches by default
- `--render-copy` — fallback path that asks Oracle to render/copy instead of sending directly

### `ief-oracle status`

Show recent repo-local Oracle sessions.

Minimum useful output:

- session id
- mode
- model
- status
- requested/completed timestamps
- artifact path

### `ief-oracle session <id>`

Print the stored session artifact paths and optionally render the saved response.

## Default behavior

### Session folder

Default location:

```text
memory/oracle-sessions/<session-id>/
```

### Session id generation

If not provided, generate from:

- ISO-like timestamp
- short slug from the prompt or user note

Example:

```text
2026-03-09-architecture-review
```

### Files written before execution

Before Oracle is invoked, create:

- `request.md`
- `meta.json`

This ensures there is a durable artifact even if the Oracle run fails.

### Files written after execution

After Oracle completes or is captured later:

- `response.md`
- update `meta.json` status/timestamps
- optional `notes.md`

## Mode selection policy

### `auto`

Recommended first policy:

- choose `browser` when no explicit API need is present
- choose `api` only when explicitly requested or when automation requires it

Reason:

IEF's default target user is likely to already have ChatGPT Plus, and browser mode avoids API billing.

### `browser`

Recommended defaults:

- manual-login profile reuse
- repo-local artifacts always on
- explicit note in metadata that billing is ChatGPT-side, not API-side

### `api`

Recommended defaults:

- prefer `gpt-5.4` over Pro unless explicitly requested
- include cost notes in `meta.json`
- preserve the exact model used so billing trade-offs remain auditable

## Suggested metadata shape

```json
{
  "sessionId": "2026-03-09-architecture-review",
  "source": "ief-oracle",
  "oracleCommand": "npx -y @steipete/oracle --engine browser ...",
  "mode": "browser",
  "model": "gpt-5.4",
  "status": "completed",
  "requestedAt": "2026-03-09T12:00:00Z",
  "completedAt": "2026-03-09T12:11:00Z",
  "artifactDir": "memory/oracle-sessions/2026-03-09-architecture-review",
  "costNotes": "browser-mode via ChatGPT Plus"
}
```

## Logging expectations

When `ief-oracle ask` produces a meaningful result, the IEF loop should still:

- add a concise entry to `LOG.md`
- update `TODO.md` if the Oracle result changes next actions

The wrapper should help with artifact creation, but should not replace the normal IEF memory loop.

## Non-goals

The first wrapper should **not** try to:

- replace Oracle's own session engine
- support every provider from day one
- add a registry or marketplace
- hide all Oracle complexity behind a large abstraction layer

## Minimal implementation strategy

If this moves from spec to code, the first implementation should:

1. be a very small script or CLI
2. shell out to `npx -y @steipete/oracle`
3. create/update repo-local artifacts around the call
4. stay browser-first by default

That is enough to make Oracle an IEF-native workflow without building a second Oracle.
