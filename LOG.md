# Interaction Log

Template:
- Date – [Short description of item].
  - Outcome: [What was done].
  - Learning (optional): [What was discovered].

## Entries

- 2026-03-15 – Researched cross-agent standards for skills and instruction files.
  - Outcome: Added `memory/SkillLoadingStandardsResearch.md`, concluding that there is no universal standard, but there are now two meaningful emerging conventions: `AGENTS.md` for persistent repo instructions and Agent Skills / `SKILL.md` for reusable capability packs. The note also captures OpenClaw’s explicit loading rules and compares Codex, Claude Code, and GitHub Copilot.
  - Learning: The important design distinction is not “which one file wins,” but “repo instructions vs reusable skills.” IEF should keep `AGENTS.md` for repo behavior and align its reusable skills with the portable Agent Skills subset instead of inventing a private format.

- 2026-03-15 – Researched concrete OpenClaw skills for IEF with an Oracle-style escalation.
  - Outcome: Added `memory/OpenClawConcreteSkillsForIEF.md` with a ranked shortlist of the most relevant concrete OpenClaw skills for IEF (`github`, `skill-creator`, `prose`, `clawhub`, `healthcheck`, `obsidian`) plus secondary candidates, and created `memory/oracle-sessions/2026-03-15-openclaw-concrete-skills/` to record the Oracle request, the blocked execution state, and the fallback notes.
  - Learning: The most useful immediate OpenClaw additions for IEF split into three layers: execution (`github`), skill-authoring discipline (`skill-creator`), and future orchestration (`prose`). Browser-first Oracle was still useful as a framing/workflow tool, but true execution from this container is blocked without browser auth or an API key; additionally, `--copy-markdown` also failed here because `xsel` is missing, so `--render-markdown` was the workable fallback.

- 2026-03-15 – Researched how OpenClaw could extend IEF.
  - Outcome: Added `memory/OpenClawForIEF.md`, positioning OpenClaw as a possible always-on control plane around IEF rather than an IDE replacement, summarizing the most relevant capabilities for IEF (remote access, scheduling, multi-agent routing, workspace memory, tool orchestration, and skills), and answering the subscription question: OpenAI Codex OAuth is a supported subscription path, while Anthropic subscription auth is technically possible via setup-token but more caveated than API keys.
  - Learning: OpenClaw becomes attractive for IEF only when reachability, scheduling, and orchestration matter; for the direct repo coding loop, dedicated coding agents remain the better default.

- 2026-03-15 – Summarized the always-on memory review into an IEF-oriented recommendation.
  - Outcome: Reworked `memory/AlwaysOnMemoryAgentReview.md` into a concise follow-up note, answered the open questions about consolidation prompts, SQLite vs repo files, summarization vs consolidation, Mode C, delta vs whole-repo inputs, OpenClaw similarities, and renamed the concept for IEF as a “repo memory refinement loop”.
  - Learning: For IEF, the valuable reusable idea is not an always-on daemon but a repo-native refinement loop: capture, summarize or consolidate deliberately, then answer later from durable memory artifacts.

- 2026-03-15 – Resolved Oracle feedback and simplified the current direction.
  - Outcome: Added `memory/OracleCurrentDirection.md` as the concise current Oracle reference, updated the Oracle skill/templates to remove cost-driven guidance, explicitly scoped v1 to ChatGPT browser mode with API fallback, documented that the request/session layout is an IEF convention inspired by Oracle, and clarified that API keys kept in the environment are not automatically hidden from an agent that can execute commands there.
  - Learning: The cleanest v1 stance is to use `@steipete/oracle` directly, keep the repo-local artifact conventions in IEF, and treat a thin `ief-oracle` wrapper as an optimization only if repeated friction appears.

- 2026-03-15 – Added explicit tool-problem reporting guidance.
  - Outcome: Updated `AGENTS.md` to require agents to report missing or failing tools, binaries, services, or environment capabilities together with the impact on the task and any workaround attempted.
  - Learning: Silent tool failure hides important execution constraints; IEF works better when environment limitations are surfaced as first-class project context.

- 2026-03-15 – Simplified README and skills layout guidance.
  - Outcome: Updated `AGENTS.md` to be the single authoritative source for control-plane and memory layout, simplified `README.md` to point there instead of repeating the same structure, and trimmed `skills/README.md` to reference `AGENTS.md` rather than duplicating layout details.
  - Learning: For IEF, the structure is easiest to maintain when `AGENTS.md` owns the operational contract and the other docs only point back to it where possible.

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