# Oracle-style browser automation plan for IEF

## Goal

Reuse the core idea behind [steipete/oracle](https://github.com/steipete/oracle): when the main agent gets stuck, it should be able to hand a prompt plus files to a stronger external model, wait asynchronously, and pull the answer back into the repo workflow.

For IEF, the important constraint is different from Oracle's default path:

- prefer browser automation over API keys
- target subscriptions you already have: **ChatGPT Plus** and **Claude Pro**
- keep the result easy to run from an agent inside a repo-centric workflow

## What Oracle already proves

Oracle is valuable to IEF not because every file should be ported, but because it already validates the hard parts of the pattern:

1. **Prompt + file bundling works**  
   The agent can package a prompt and supporting files into one payload that another model can consume.
2. **Long-running sessions need durable state**  
   Stronger reasoning runs can take many minutes; session IDs, logs, and restart/reattach support matter.
3. **Browser mode is viable**  
   Oracle already runs ChatGPT in the browser and has mature handling for profile reuse, cookie sync, manual login, attachments, and reattach.
4. **Provider-specific DOM adapters are the right boundary**  
   ChatGPT and Gemini are different UIs, but the surrounding session machinery stays mostly the same.

## Oracle implementation areas worth studying

If this turns into code work, these parts of Oracle are the most useful reference points:

- `src/browser/` — Chrome lifecycle, prompt submission, response capture, profile reuse, and reattach patterns
- `src/browser/providers/` — provider-specific DOM adapters; this is the clearest template for a future Claude web adapter
- `src/browser/providerDomFlow.ts` — the right abstraction point between shared browser flow and provider-specific DOM behavior
- `src/cli/dryRun.ts` and browser prompt assembly helpers — useful shape for previewing bundled prompts before launching a browser run
- session/runtime metadata handling and reattach support — useful for durable long-running runs, especially for "thinking" models
- `docs/browser-mode.md` — operational lessons, limitations, and follow-up ideas already discovered the hard way

## Parts worth reusing conceptually

The following Oracle concepts map well to IEF and should be reused either directly or by reimplementing the same structure:

### 1. Prompt assembly

Keep one component that:

- accepts the user prompt
- optionally adds system instructions
- resolves attached files
- decides whether to inline file contents or upload them
- records rough token / size estimates for operator visibility

This is the piece that closes the loop for agent escalation.

### 2. Session store

Keep a durable session record with at least:

- session ID
- requested provider/model
- prompt metadata
- attached files
- run status: queued / running / completed / failed
- timestamps
- raw response artifact
- short summary for quick inspection
- reattach hints for browser-backed runs

For IEF, session artifacts should remain easy to link from `LOG.md` and future TODO items.

### 3. Browser runtime lifecycle

Oracle already shows the important operational patterns:

- persistent manual-login profile
- optional cookie sync
- headful fallback when login/anti-bot friction appears
- keep-browser and reattach flows for long reasoning runs
- robust capture of the final assistant turn

These patterns are more important than any single selector.

### 4. Provider adapter boundary

Use a common contract like:

- `prepareSession()`
- `openConversation()`
- `submitPrompt()`
- `waitForCompletion()`
- `captureResponse()`
- `captureMetadata()`

Each provider should own only its DOM specifics.

## What should change for IEF

### Browser-first instead of API-first

Oracle supports both API and browser engines. For IEF, the first useful version should be explicitly browser-first:

- **ChatGPT Plus** via chatgpt.com
- **Claude Pro** via claude.ai

That keeps the tool aligned with your existing paid subscriptions and avoids API billing/credential management.

### Smaller initial scope

Oracle is a full product. IEF only needs the subset that unlocks escalation:

- submit prompt + files
- list sessions
- fetch latest answer
- reattach to a live browser run
- optionally restart a failed/timed-out session

TUI, multi-provider fan-out, cost estimation, and polished distribution can wait.

### Repo-native output

For IEF, the output should land in repo-friendly artifacts, for example:

- `memory/oracle-sessions/<session-id>/request.md`
- `memory/oracle-sessions/<session-id>/response.md`
- `memory/oracle-sessions/<session-id>/meta.json`

That fits the "repo as shared memory" model better than a hidden global cache.

## Recommended MVP architecture

### Phase 1 — ChatGPT Plus only

Build the smallest useful path first:

- CLI command such as `ief-oracle ask`
- persistent Chrome profile with manual login
- prompt bundling with inline files first, upload fallback second
- save response markdown to repo-local session folder
- log the session into `LOG.md`

Why first: Oracle already demonstrates that ChatGPT browser automation is the most mature reusable path.

### Phase 2 — durable long-run support

Add:

- `ief-oracle status`
- `ief-oracle session <id>`
- reattach to existing browser session/tab
- timeout handling with resumable state

Why second: this is what turns a neat demo into something an agent can depend on.

### Phase 3 — Claude Pro adapter

Add a second browser provider with the same interface:

- dedicated persistent profile
- Claude-specific prompt box and attachment handling
- response capture
- optional project selection if Claude Projects become relevant

Important: keep Claude isolated behind its own adapter so DOM drift does not affect ChatGPT runs.

### Phase 4 — agent integration

Expose one high-level agent action:

- "ask the oracle"

Inputs:

- question / task
- optional file globs
- preferred provider: `chatgpt` | `claude` | `auto`
- priority / patience mode

Outputs:

- session ID
- current status
- final answer path
- concise summary for insertion into the active work log

## Provider notes

### ChatGPT Plus

Best starting point.

Why:

- Oracle already has a mature browser automation path for ChatGPT
- long reasoning runs are a known use case
- attachments and copy-response flows are already proven patterns
- manual-login persistent profiles remove most keychain/cookie friction

Main risks:

- model picker DOM drift
- anti-bot or login interruptions
- attachment UX changes

### Claude Pro

Worth adding, but second.

Why:

- gives IEF a second opinion / alternative reasoning style
- maps well to the same session-oriented escalation concept

Expected extra work:

- new selectors and response extraction logic
- attachment handling may differ from ChatGPT
- rate limits / product UI changes may be less documented
- reattach behavior will need fresh experimentation

## Recommended implementation stance

**Reuse the architecture, not the whole product surface.**

Practical recommendation:

1. Reuse Oracle's design ideas directly.
2. Borrow code selectively only where licensing and maintenance make sense.
3. Keep the IEF version smaller and repo-local.
4. Add Claude only after ChatGPT session durability works well.

This minimizes scope while preserving the main unlock.

## Why this fits IEF specifically

IEF is already built around:

- a durable repo memory
- explicit TODO execution
- progress logging
- human review after meaningful agent work

An Oracle-style escalation tool fits naturally as:

- a fallback when the main agent is stuck
- a research accelerator for broad web scans
- a way to capture stronger long-form reasoning as repo artifacts
- a repeatable mechanism instead of ad hoc copy/paste into web UIs

## Suggested first implementation target

If this moves from research into code, start with this exact target:

> A repo-local `ief-oracle ask` command that opens a persistent ChatGPT browser session, submits a bundled markdown prompt with optional files, stores request/response artifacts under `memory/oracle-sessions/`, and supports later reattach/status inspection.

That is the smallest version that meaningfully improves the IEF loop.

## Open questions before coding

1. Should IEF keep oracle sessions per repo or in one shared workspace location?
2. Should the main agent write escalation requests automatically, or only when explicitly prompted?
3. Is Claude Pro a required provider for v1, or is ChatGPT Plus enough for the first usable version?
4. Do you want the eventual implementation to live inside this repo as a reference spec, or inside a separate tool repo?
