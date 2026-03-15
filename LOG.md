# Interaction Log

Template:
- Date – [Short description of item].
  - Outcome: [What was done].
  - Learning (optional): [What was discovered].

## Entries

- 2026-03-15 – Reviewed TODO and LOG workflow options.
  - Outcome: Added `memory/TodoAndLogWorkflow.md`, recommending a hybrid model: keep `TODO.md` and `LOG.md` separate at the top level, but allow an optional `todo/` directory with one markdown file per larger task and optional task-local logs.
  - Learning: The scalable compromise is not one giant combined TODO+LOG file, but a short root queue plus richer per-task files when the work justifies them.

- 2026-03-15 – Added MIT licensing to IEF.
  - Outcome: Added an MIT [LICENSE](LICENSE) file and linked it from the README so the repository now has an explicit default reuse license.
  - Learning: For a framework repo like IEF, making the license explicit early removes ambiguity for later reuse of docs, skills, and reference implementations.

- 2026-03-09 – Created always-on agents lab project.
  - Outcome: Created `/workspaces/workplace/always-on-agents-lab` with an initial `README.md`, `LOG.md`, `TODO.md`, a pointer `AGENTS.md`, and `memory/RemoteAccessAndAgentSetupReview.md` capturing the first recommendation: prefer `code-server` + `tmux` + repo memory as the main always-on agent surface, use Oracle-style remote browser services for browser-backed escalation, and treat Guacamole/Kasm as full-desktop fallbacks.
  - Learning: For long-running coding agents, the most practical “connect from anywhere” architecture is usually browser-native code access plus persistent terminal sessions, not a browser-streamed desktop as the primary workspace.

- 2026-03-09 – Moved TODO and LOG to repo root.
  - Outcome: Promoted `LOG.md` and `TODO.md` to the repo root, updated `AGENTS.md`, `README.md`, and related skill/research references, and reserved `memory/` for supporting notes, research, and session artifacts.
  - Learning: `README.md`, `LOG.md`, and `TODO.md` work better as a visible repo control plane, while `memory/` works better as the backing store for larger execution artifacts.

- 2026-03-09 – Always-on memory agent pattern review for IEF.
  - Outcome: Added `memory/AlwaysOnMemoryAgentReview.md`, summarizing the Google always-on memory agent as a three-loop pattern (capture, consolidate, query), explaining how IEF can adapt it as a repo-native workflow, and clarifying that the concept can be reused without API keys but not as cleanly as a true unattended service.
  - Learning: The most important reusable idea is not “automatic summary generation,” but a durable ingestion-plus-consolidation layer that turns many inputs into queryable project memory.

- 2026-03-09 – Oracle integration foundations executed.
  - Outcome: Added the first repo-level Oracle integration artifacts: `skills/README.md`, `skills/oracle-consult/SKILL.md`, Oracle request/session templates, `memory/oracle-sessions/README.md`, `memory/OracleWrapperSpec.md`, and README guidance so Oracle is now represented as a concrete reusable IEF workflow.
  - Learning: The smallest useful Oracle integration in IEF is not runtime code first; it is a reusable skill plus a durable artifact contract that makes future automation safe and predictable.

- 2026-03-09 – Oracle integration roadmap.
  - Outcome: Added `memory/OracleIntegrationRoadmap.md`, converting the earlier Oracle research into phased integration work for IEF and identifying the immediate in-repo steps: add `skills/`, define an Oracle skill, standardize session artifacts, and update the README.
  - Learning: For IEF, the right first move is to make Oracle a first-class repo workflow before attempting to build a dedicated runtime wrapper.

- 2026-03-09 – Oracle API cost review vs ChatGPT Plus.
  - Outcome: Added `memory/OracleApiCostNotes.md` documenting the current billing split between ChatGPT Plus and the OpenAI API, plus practical Oracle cost examples for `gpt-5.4`, `gpt-5.4-pro`, and `gpt-5-mini`.
  - Learning: For IEF, browser-first Oracle integration is not only simpler for users with ChatGPT Plus, but usually much cheaper than defaulting to Pro/API runs.

- 2026-03-08 – OpenClaw skills review for IEF.
  - Outcome: Added `memory/OpenClawSkillsReview.md`, summarizing OpenClaw's skills model and extracting the parts most worth adapting to IEF: markdown-first skill folders, repo-vs-global precedence, eligibility gating, compact prompt injection, and explicit trust boundaries.
  - Learning: The highest-leverage idea is to treat skills as structured reusable instructions living in Git, not as a heavyweight plugin marketplace.

- 2026-03-08 – Steipete repository review for IEF.
  - Outcome: Reviewed Peter Steinberger's public GitHub repositories, created `memory/SteipeteRepoReview.md`, and ranked the repos most relevant to IEF concepts such as repo memory, browser automation, reusable agent docs, skills, remote automation, and collaboration loops.
  - Learning: The strongest reusable ideas are session/checkpoint artifacts, portable shared agent instructions/helpers, and opt-in automation adapters rather than one monolithic agent system.

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