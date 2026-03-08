# Steipete Repositories Relevant to IEF

## Scope
Reviewed Peter Steinberger's public GitHub repositories on 2026-03-08 using repository metadata and README content.

Focus areas:
- agent workflows
- repo memory
- browser automation
- session management
- docs-for-agents
- skills
- CLI-first tooling
- remote automation
- human/agent collaboration loops

## Ranked Shortlist

1. **steipete/cli** — Entire CLI captures AI agent sessions alongside Git work using hooks, checkpoints, rewind/resume, and a separate metadata branch.
   - **Why it matters for IEF:** This is the closest public implementation to “repo as execution memory,” especially the ideas of session capture, traceability, explainability, rewind, and resumable work.
   - **Caution:** It is hook-heavy and stores metadata on a shadow branch (`entire/checkpoints/v1`) rather than in visible repo files, so it is more operationally complex than IEF’s simpler memory-in-repo approach.

2. **steipete/agent-scripts** — Shared cross-repo guardrails, AGENTS instructions, skills, docs helpers, browser helpers, and a commit helper.
   - **Why it matters for IEF:** Strong reference for reusable docs-for-agents, portable helper scripts, pointer-style AGENTS usage, and lightweight skills distribution across many repos.
   - **Caution:** The workflow is opinionated around one canonical shared repo and exact mirroring between downstream repos.

3. **steipete/oracle** — CLI that bundles prompt + files and sends them to other models via API, browser automation, MCP, or manual copy/paste, with session tracking and follow-up lineage.
   - **Why it matters for IEF:** Best source for stuck-escalation patterns, browser-first fallback, session lineage, remote browser service, AGENTS integration, and skill packaging.
   - **Caution:** Browser mode is explicitly experimental, and session storage lives outside the working repo by default.

4. **steipete/claude-code-mcp** — One-shot MCP server that lets one agent delegate work to Claude Code.
   - **Why it matters for IEF:** Useful concrete pattern for “agent in the agent,” especially when the primary agent needs a second implementation path or a fresh pass on a blocked task.
   - **Caution:** It deliberately bypasses permission prompts and grants a powerful secondary agent broad system access.

5. **steipete/research** — Public scratchpad repo for async coding-agent experiments with isolated folders/branches per question.
   - **Why it matters for IEF:** Good template for research-oriented IEF repos: focused questions, explicit expected artifacts, asynchronous review, and reusable experiment reports.
   - **Caution:** The repo assumes a more disposable, high-freedom research posture than a production or long-lived project repo.

6. **steipete/macos-automator-mcp** — MCP server for AppleScript/JXA with a large built-in automation recipe library.
   - **Why it matters for IEF:** Strong example of a remote-automation adapter with a knowledge base behind it instead of only raw command execution.
   - **Caution:** macOS-only, high-permission, and dependent on system automation/accessibility approvals.

7. **steipete/canvas** — Lightweight CLI + controlled Chromium workspace for agent-created HTML/JS, DOM inspection, screenshots, and local browser validation.
   - **Why it matters for IEF:** Interesting browser-workbench pattern for agent-driven UI iteration and verification without needing a full remote browser stack.
   - **Caution:** The README says the idea is now mainly being iterated elsewhere (`clawdis`), so this repo may not be the long-term home of the concept.

8. **steipete/CodeLooper** — Menubar watchdog that notices when Cursor stops or loses flow and tries to recover the agent loop.
   - **Why it matters for IEF:** Valuable concept for loop resilience, watchdogs, and unattended execution recovery.
   - **Caution:** Archived, macOS-specific, and the README notes that it does not yet work.

## Watchlist / Secondary Repositories
- **steipete/notarium-mcp** — interesting for external memory and note sync patterns; less aligned with IEF’s repo-first memory model.
- **steipete/iterm-mcp** — useful for shared terminal session control and REPL interaction.
- **steipete/brabble** — interesting for low-friction human-to-agent trigger hooks via local voice input.
- **steipete/RepoBar** — good inspiration for glanceable repo state, CI state, and local Git/worktree visibility.
- **steipete/agent-rules** — historical context only; archived and explicitly superseded by `agent-scripts`.

## Broader Patterns Relevant to IEF

### 1. Session memory is often stored beside the work, not only in chat
Across `cli`, `oracle`, `brabble`, and `notarium-mcp`, the recurring idea is that transcripts, checkpoints, logs, or cached notes become durable artifacts with IDs, storage locations, and replay value.

### 2. Reusable agent instructions should be treated like shared infrastructure
`agent-scripts` strongly suggests that AGENTS text, docs helpers, commit helpers, and skills work better when they are portable, explicit, and maintained as reusable building blocks.

### 3. Browser and desktop automation are pragmatic fallback layers
`oracle`, `canvas`, `macos-automator-mcp`, and `CodeLooper` all reflect a pattern of using UI automation when APIs are missing, blocked, or lower leverage than direct interface control.

### 4. Small capability adapters beat one giant agent system
Instead of one monolith, the repos trend toward focused adapters: second-agent delegation, browser control, terminal control, note bridges, repo-status views, voice hooks, and desktop automation.

### 5. High-power automation is paired with explicit human oversight
Many READMEs call out danger, permissions, experimental status, manual login, or the need for monitoring. The common pattern is not “full autonomy,” but “high autonomy with clear human checkpoints and recovery tools.”

## IEF-Specific Implications
- IEF should continue to emphasize durable repo memory, but can borrow the idea of explicit session IDs, resume points, and rewind-friendly artifacts.
- IEF could benefit from a lightweight shared helper/skills layer similar to `agent-scripts`, but likely without depending on one exact mirrored external repo.
- Oracle-like escalation is a strong complement to IEF: when the main agent is stuck, bundle repo context and escalate to another model or browser-backed provider.
- Remote/browser/desktop automation in IEF should stay adapter-based and opt-in, with very explicit safety language.
- A future IEF research companion repo could adopt the `research` style: one folder per question, explicit prompt, reproducible artifact, clear outcome.
