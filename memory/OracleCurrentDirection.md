# Oracle current direction for IEF

This is the concise current direction for Oracle integration in IEF.

Source notes:

- `memory/OracleBrowserAutomationPlan.md`
- `memory/OracleIntegrationRoadmap.md`
- `memory/OracleWrapperSpec.md`

Use this file as the day-to-day reference.

## Current v1 decisions

1. **Use `@steipete/oracle` directly first**
   - do not reimplement Oracle inside IEF
   - treat Oracle itself as the execution engine
2. **Keep `ief-oracle` as an optional later wrapper**
   - only build it if repeated operational friction justifies it
3. **ChatGPT browser mode is the only browser target for now**
   - Claude.ai browser automation is out of scope for v1
4. **API mode is the fallback path**
   - use it if browser mode is unreliable or if more scripted execution is needed
5. **Oracle sessions should stay per repo for now**
   - `memory/oracle-sessions/<id>/`
   - this is the simpler default
6. **Oracle use can be autonomous**
   - the main agent may decide to escalate when the task justifies it
7. **Keep the integration in this repo for now**
   - skills, docs, and conventions live here until a standalone tool is clearly warranted

## Three implementation approaches

## Option 1 — use `@steipete/oracle` directly

### What it means

Use Oracle as-is from IEF workflows:

- `npx -y @steipete/oracle --engine browser ...`
- `npx -y @steipete/oracle --engine api ...`

IEF adds only:

- repo-local request/response artifacts
- skill guidance
- log / TODO integration

### Pros

- simplest path
- fastest path to real usage
- benefits from upstream Oracle improvements immediately
- avoids maintaining a parallel browser automation stack

### Cons

- some IEF-specific conventions remain manual
- CLI flags and Oracle behavior remain Oracle-shaped, not IEF-shaped
- repo-local artifacts are layered around Oracle, not native to Oracle itself

### Recommendation

**Use this now.**

This is the best current path.

## Option 2 — thin `ief-oracle` wrapper that calls Oracle

### What it means

Add a very small helper that:

- writes repo-local request artifacts
- calls `@steipete/oracle`
- stores response/meta in the expected repo structure

### Pros

- smoother IEF-native UX
- deterministic repo-local artifacts
- can hide repetitive boilerplate

### Cons

- still another thing to maintain
- wrapper drift becomes possible when Oracle changes
- not necessary until real usage pain appears repeatedly

### Recommendation

**Keep as a later step, not a v1 requirement.**

## Option 3 — reimplement Oracle behavior inside IEF

### What it means

Build a new browser/API escalation tool from scratch inside IEF.

### Pros

- full control
- fully IEF-shaped UX

### Cons

- highest maintenance burden
- duplicates a working upstream tool
- unnecessary scope for the current repo

### Recommendation

**Out of scope.**

## Why the current direction changed

The earlier phrase “reuse the architecture, not the whole product surface” was useful at research time, but the practical v1 decision is simpler:

- **reuse Oracle directly first**
- **reuse only as much IEF-specific wrapper logic as needed later**

So this is not a contradiction.

It is a refinement:

- research posture: do not blindly copy the whole Oracle product into IEF
- implementation posture: use Oracle directly where it already solves the hard problem well

## Scope for v1

### In scope

- ChatGPT browser mode
- OpenAI API fallback
- autonomous escalation decisions
- repo-local Oracle artifacts
- Oracle skill guidance

### Out of scope

- Claude.ai browser automation
- multi-provider browser fan-out
- full standalone Oracle clone inside IEF
- complex secrets/config abstractions

## When Oracle should be used

Oracle is especially valuable when:

- the agent is stuck after meaningful local investigation
- a second opinion on architecture or debugging is needed
- the task depends on **high-quality web research** or synthesis across many sources
- the result should become a durable repo artifact for later reuse

This last point matters: Oracle is not only for debugging. It is also useful when research quality matters more than speed.

## Origin of the request/session layout

The repo-local Oracle request template and session layout are **IEF conventions**.

They are:

- inspired by Oracle's session-oriented workflow
- compatible with using Oracle as the execution engine
- not copied from Oracle's exact on-disk structure

That means IEF owns these repo-local artifact conventions.

## API mode and secret handling

## Where the API key goes

For API mode, the normal pattern is an environment variable such as:

- `OPENAI_API_KEY`

Keep it:

- outside the repo
- outside committed files
- preferably in shell config, a secrets manager, or an operator-managed runtime environment

## Is it safe from the agent?

**Not automatically.**

Important distinction:

- safe from Git history and repo leakage: **yes**, if kept outside the repo
- safe from an agent that can execute commands in that environment: **no**, not by default

If the agent can run commands in an environment that exposes `OPENAI_API_KEY`, then the agent can potentially use that key.

So if you want the key to be unavailable to the agent, you need a stricter boundary such as:

- browser mode instead of API mode
- human-mediated execution
- a locked-down proxy/service with narrower permissions than full shell access

## Practical conclusion on API mode

API mode is operationally useful, but it should be treated as:

- a fallback for reliability/automation
- a mode that requires deliberate secret exposure

## Recommended next steps

1. keep `@steipete/oracle` as the actual engine
2. keep the Oracle skill and artifact conventions in this repo
3. use ChatGPT browser mode first
4. use API mode only when browser mode proves too unreliable or too manual
5. build `ief-oracle` only after real usage shows repeated friction

## Plain-language conclusion

For IEF, the right Oracle path is:

- **use `@steipete/oracle` directly**
- **wrap it only lightly if needed later**
- **stay ChatGPT-browser-first for v1**
- **treat API mode as a fallback**
- **keep repo-local Oracle artifacts as an IEF convention**
