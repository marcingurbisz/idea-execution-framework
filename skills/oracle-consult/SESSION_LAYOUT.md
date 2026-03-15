# Oracle session artifact layout

The default IEF convention for Oracle outputs is:

```text
memory/
  oracle-sessions/
    <session-id>/
      request.md
      response.md
      meta.json
      notes.md          # optional
```

## Required files

### `request.md`
The exact request sent to Oracle or prepared for Oracle.

### `response.md`
The captured answer from Oracle.

### `meta.json`
Suggested fields:

```json
{
  "sessionId": "2026-03-09-example",
  "mode": "browser",
  "provider": "openai",
  "model": "gpt-5.4",
  "status": "completed",
  "requestedAt": "2026-03-09T12:00:00Z",
  "completedAt": "2026-03-09T12:12:00Z",
  "source": "oracle",
  "transportNotes": "browser-mode via signed-in ChatGPT session"
}
```

### `notes.md` (optional)
Use this for the IEF agent's interpretation of the answer, follow-up choices, or extracted next actions.

## Logging rule

Whenever an Oracle session materially affects the work, add a matching note under the relevant TODO item or its linked task file and mention the session folder.
