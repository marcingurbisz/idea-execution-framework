# Oracle request

## Task
Identify the concrete OpenClaw skills or official skill packs that would be most beneficial for the Idea Execution Framework (IEF).

Return a ranked shortlist with:
- exact skill names
- why each one fits IEF specifically
- likely adoption priority (`adopt now`, `pilot next`, `optional`)
- important caveats
- any official `.prose` examples worth copying if OpenProse is recommended

## Why escalation is needed
This task benefits from a second opinion with stronger cross-source synthesis across the OpenClaw repo, docs, and skill/plugin ecosystem. I want a durable repo artifact, not just a quick local guess.

## Current understanding
- OpenClaw looks most useful for IEF as an outer control plane, not as an IDE replacement.
- The most promising categories seem to be GitHub workflow support, multi-agent orchestration, skill discovery, durable Markdown knowledge, and host hardening for always-on deployments.
- I need concrete names and practical ranking, not just general skills-system observations.

## Relevant files
- `memory/OpenClawSkillsReview.md` — earlier generic review of the OpenClaw skills model
- `memory/OpenClawForIEF.md` — how OpenClaw could extend IEF overall
- `TODO.md` — contains the active task wording

## Constraints
- Prefer official or bundled OpenClaw skills and official skill packs before community extras.
- Focus on practical repo-centric workflows that match IEF: repo memory, TODO/LOG discipline, GitHub collaboration, orchestration, durable Markdown artifacts, and always-on host operations.
- Browser-first Oracle workflow; API fallback only if needed.

## Desired output
A concise ranked recommendation that can be turned into an IEF memory note.

## Notes for follow-up
Persist the result under `memory/`, update `LOG.md`, and mark the matching TODO complete if the result is actionable.
