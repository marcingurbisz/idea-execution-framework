# Interaction Log

Template:
- Date – [Short description of item].
  - Outcome: [What was done].
  - Learning (optional): [What was discovered].

## Entries

- 2026-03-08 – Oracle browser adaptation plan.
  - Outcome: Reviewed Peter Steinberger's Oracle concept and documented an IEF-specific plan in `memory/OracleBrowserAutomationPlan.md`, focused on repo-local session artifacts and browser-first escalation using ChatGPT Plus first and Claude Pro second.
  - Learning: The main reusable value is Oracle's session + browser + provider-adapter architecture; IEF does not need the full product surface to get the core unlock.

- 2026-03-08 – Small IEF README adjustments.
  - Outcome: Clarified that the devcontainer is optional but recommended, tightened the vision wording around chat history vs short-lived context, rewrote the usage steps to be shorter, and documented why execution memory stays under `memory/`.
  - Learning: Keeping `README.md` at repo root and `LOG.md`/`TODO.md` under `memory/` keeps the repo entry point clean without losing room for richer execution artifacts.

- 2026-03-07 – README vision refresh.
  - Outcome: Improved the README vision section to better explain IEF as a human/agent way of working and added a short summary of what `AGENTS.md` contains.

- 2026-03-07 – README usage guidance refresh.
  - Outcome: Expanded `README.md` with memory-preparation guidance, prompt examples for new and existing projects, and a short description of the usual human/agent working rhythm.
