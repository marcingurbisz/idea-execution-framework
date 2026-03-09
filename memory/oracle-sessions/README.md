# Oracle session artifacts

Store repo-local Oracle escalation runs here.

Recommended layout:

```text
memory/oracle-sessions/
  <session-id>/
    request.md
    response.md
    meta.json
    notes.md   # optional
```

Guidance:

- keep one folder per Oracle run
- prefer durable, human-readable artifacts
- log meaningful runs in `LOG.md`
- update `TODO.md` when the result changes the plan

See [../../skills/oracle-consult/SESSION_LAYOUT.md](../../skills/oracle-consult/SESSION_LAYOUT.md) for the detailed convention.
