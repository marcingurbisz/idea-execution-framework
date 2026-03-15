---
name: oracle-consult
description: Escalate a hard task to Oracle with repo-local artifacts, using ChatGPT browser mode first and API mode when browser execution is not reliable enough.
requires:
  bins: [node]
---

Use this skill when the current agent is stuck, needs a second opinion, needs stronger long-form reasoning, or should package a prompt plus files for Oracle.

## Use this skill when

- a bug/design problem remains unresolved after meaningful local investigation
- you want a second model pass on architecture, debugging, or review
- you need wide-context analysis across multiple files
- the task depends on high-quality web research or synthesis across many sources
- you want a durable research/review artifact stored in the repo

## Do not use this skill when

- the task is still cheap to solve with normal repo exploration
- the problem is mostly a missing requirement that should be clarified with the human
- the likely answer does not justify the extra execution overhead of escalation

## Current default

Prefer this order unless there is a clear reason not to:

1. **Browser mode** with ChatGPT via `@steipete/oracle`
2. **API mode** with OpenAI key when browser mode is unreliable or a more scripted path is needed

Current v1 scope:

- ChatGPT browser mode is in scope
- Claude.ai browser mode is out of scope for now
- future wrappers are optional; direct Oracle usage is the current default

## Required artifact layout

Before running Oracle, create a session folder under:

`memory/oracle-sessions/<session-id>/`

Expected files:

- `request.md` — the exact escalation request
- `response.md` — the captured Oracle answer
- `meta.json` — mode, model, timestamps, command notes, status
- optional `notes.md` — your local interpretation or follow-up decisions

See `SESSION_LAYOUT.md` and `REQUEST_TEMPLATE.md` in this skill folder.

## Recommended workflow

1. Summarize the problem in your own words.
2. Select only the files that materially help Oracle answer well.
3. Write `request.md` using the request template.
4. Choose browser mode first if ChatGPT Plus is enough.
5. Run Oracle or prepare a `--render --copy` fallback.
6. Save the response into `response.md`.
7. Record the escalation in `LOG.md`.
8. Update `TODO.md` if the Oracle result changes the plan.

## Browser-first command pattern

Use a browser-oriented flow like:

- `npx -y @steipete/oracle --engine browser --browser-manual-login -p "..." --file ...`
- if automation is blocked, use `--render --copy` and keep the same repo-local artifacts

## API-mode command pattern

Use API mode when browser mode is not reliable enough or when you need a more scripted path, for example:

- `OPENAI_API_KEY=... npx -y @steipete/oracle --engine api --model gpt-5.4 -p "..." --file ...`

Prefer non-Pro models by default unless the task truly needs Pro-level reasoning.

## Result handling

After Oracle returns:

- distill the answer into actionable repo updates
- keep the full Oracle answer as an artifact rather than overwriting it with a short summary only
- note whether Oracle changed the direction, confirmed the plan, or only added confidence

## References

- `memory/OracleCurrentDirection.md`
- `memory/OracleBrowserAutomationPlan.md`
- `memory/OracleIntegrationRoadmap.md`
