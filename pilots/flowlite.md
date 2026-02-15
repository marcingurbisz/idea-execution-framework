# Pilot: FlowLite

Goal: validate Idea Execution Framework on a real codebase with tests, by delivering small, auditable changes.

## Scope rules
- Prefer small, reviewable diffs.
- Keep changes test-driven or at least test-verified (`./gradlew test`).
- Persist: update memory after each meaningful loop (Decision/Learning/Log/TODO).

## Loop (one iteration)
1) Pick one small task from `memory/TODO.md` (or FlowLite TODO).
2) Clarify constraints (public API changes? backwards compat? naming rules?).
3) Implement in FlowLite.
4) Run `./gradlew test` in FlowLite.
5) Update IEF memory:
   - `memory/LOG.md`: what changed + outcome.
   - `memory/DECISIONS.md`: only if you made a durable choice.
   - `memory/LEARNINGS.md`: only if something is broadly reusable.
   - `memory/TODO.md`: next actions.

## Validation tasks already done (reference)
- Implemented `HistoryStore` contract and engine emissions.
- Enforced PascalCase enums.
