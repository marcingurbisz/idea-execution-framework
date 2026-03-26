# Concrete OpenClaw skills that could benefit IEF

## Scope

This note answers a narrower follow-up question than `memory/OpenClawSkillsReview.md`:

> Which concrete OpenClaw skills or official skill packs look most useful for the Idea Execution Framework right now?

The focus here is practical IEF fit:
- repo-centric execution
- TODO/LOG discipline
- GitHub collaboration
- durable Markdown artifacts
- orchestration around repo work
- always-on host operations when relevant

## Oracle-status note

I prepared an Oracle browser-mode escalation for this task under:

`memory/oracle-sessions/2026-03-15-openclaw-concrete-skills/`

A full Oracle run was not possible from this container:
- no signed-in ChatGPT browser cookies were available in the default Chrome/Chromium paths checked from this environment
- `OPENAI_API_KEY` was unset, so API fallback was unavailable
- `--copy-markdown` also hit a local tooling gap because `xsel` was missing

So this document is a **local synthesis using the Oracle workflow as a prompt/recording discipline**, plus direct OpenClaw repo/docs evidence.

## Short recommendation

If IEF wants concrete OpenClaw capabilities, the best shortlist is:

### Adopt now
- `github`
- `skill-creator`

### Pilot next
- `prose` (OpenProse skill pack)
- `clawhub`

### Context-dependent
- `healthcheck`
- `obsidian`

## Ranked shortlist

## 1. `github` — adopt now

### Why it fits IEF

This is the most obvious direct fit because IEF work already lives in Git repos and regularly touches:
- pull requests
- issues
- review comments
- CI runs
- release or repo metadata

OpenClaw's `github` skill is explicitly aimed at `gh`-based GitHub operations such as PR status, CI checks, issue actions, review workflows, and API queries.

For IEF, this would complement the main coding loop well:
- use repo-local agents for code and file changes
- use `github` when the task shifts to PRs, checks, issue triage, or review context

### Where it helps most

- post-TODO status checks
- CI failure inspection
- PR review follow-up
- issue triage tied to repo memory
- generating concise repo-status summaries for `LOG.md`

### Caveats

- requires `gh` auth to be configured
- does not replace local `git` usage
- does not replace direct code reading/review when the real task is in the files

## 2. `skill-creator` — adopt now

### Why it fits IEF

IEF already has a `skills/` directory and is actively refining skill conventions.
The OpenClaw `skill-creator` skill is a strong match because it is about:
- creating new skills
- improving existing `SKILL.md` files
- cleaning up skill directories
- validating against the AgentSkills-style layout
- keeping skill content concise and split sensibly across `SKILL.md`, `references/`, `scripts/`, and `assets/`

This is valuable for IEF because the immediate problem is not only “which skills should exist,” but also “how should IEF author and maintain them cleanly?”

### Why it is more important than it looks

The skill captures good discipline that maps well to IEF:
- keep the top-level skill short
- put detailed reference material in separate files
- treat token budget as a real resource
- design skills around concrete workflows and reusable assets

That makes it a good reference skill even if IEF never runs OpenClaw directly.

### Caveats

- this is a **meta-skill** for authoring skills, not a day-to-day execution skill
- it helps most when IEF continues investing in `skills/` as a first-class layer

## 3. `prose` (OpenProse skill pack) — pilot next

### Why it fits IEF

The `prose` skill pack activates around `.prose` workflows and OpenProse commands, and is explicitly about multi-agent orchestration.

That makes it interesting for IEF when the work stops being a single interactive repo loop and becomes:
- repeatable multi-agent research
- explicit review pipelines
- approval-aware workflows
- orchestrated sub-task execution

In other words, `prose` is not the best first addition to IEF, but it is the most interesting path if IEF wants a workflow runtime around the repo-memory model.

### Specific OpenProse examples worth borrowing

From the official examples, the patterns most relevant to IEF are:
- `33-pr-review-autofix.prose`
- `36-bug-hunter.prose`
- `38-skill-scan.prose`
- `39-architect-by-simulation.prose`
- the “captain's chair” persistent-orchestrator examples

These are useful because they show concrete shapes for:
- automated review/fix loops
- deliberate bug-hunting passes
- skill discovery/evaluation
- persistent coordinator patterns

### Caveats

- OpenProse is more infrastructure and ceremony than IEF needs for simple repo tasks
- plugin enablement and tool allowlists matter
- it is best treated as a second-phase orchestration layer, not as the starting point

## 4. `clawhub` — pilot next

### Why it fits IEF

If IEF wants a broader skills ecosystem, `clawhub` is the concrete discovery and distribution path.
The skill is built around searching, installing, updating, and publishing skills from the public registry.

This matters to IEF in two ways:
1. it gives a concrete model for how reusable skills can be discovered and versioned
2. it provides a possible future path for publishing/importing IEF-compatible skill folders

### Where it helps most

- discovering existing skills before reinventing them
- installing proven skills into a workspace
- backing up or publishing reusable skills
- treating skill folders as versioned artifacts rather than ad hoc prompts

### Caveats

- registry/distribution is not the first problem IEF needs to solve
- public skills introduce trust and review concerns
- this becomes more useful after IEF's own local skill conventions stabilize

## 5. `healthcheck` — context-dependent, but strong for always-on setups

### Why it fits IEF

The `healthcheck` skill is specifically about host hardening, risk posture, firewall/SSH/update checks, cron-based periodic checks, and version-status review for machines running OpenClaw.

That is not core to day-to-day repo execution, but it becomes highly relevant if IEF moves toward:
- an always-on assistant host
- VPS/home-server execution
- scheduled loops
- remote access experiments like the ones in `always-on-agents-lab`

### Where it helps most

- hardening a dedicated agent host
- regular environment audits
- checking exposure before enabling remote access or automation
- making “always-on agent” work more operationally responsible

### Caveats

- operational rather than repo-core
- only worth adopting when the infrastructure layer actually exists

## 6. `obsidian` — optional, mostly for Markdown interop

### Why it fits IEF

The `obsidian` skill works with Obsidian vaults as plain Markdown notes.
That makes it relevant only if the human wants IEF memory to interoperate with a broader Markdown note system outside the repo.

Potential use:
- move supporting research between repo memory and a personal knowledge base
- reuse Markdown workflows across project and personal notes
- automate note maintenance with a tool built for a Markdown vault

### Caveats

- easy to create split-brain between repo memory and external notes
- weaker fit than repo-local files as the IEF source of truth
- should stay optional, not core

## Secondary candidates

## `gh-issues`

This is interesting if IEF wants a more automated issue-to-PR loop with spawned sub-agents and review-follow-up behavior.
It feels like a **later-stage automation skill**, not a first-wave adoption item.

## `gog`

Useful only if IEF expands beyond repos into Gmail/Calendar/Docs/Drive coordination.
That is more “personal operating layer around IEF” than “core IEF repo workflow”.

## What this suggests for IEF

## Best immediate imports to learn from

1. `github` for repo-adjacent GitHub execution
2. `skill-creator` for writing better IEF skills
3. `prose` for future orchestration experiments

## Best immediate design takeaway

OpenClaw is most useful to IEF not because of one magical skill, but because it separates three concerns cleanly:
- **execution skills** like `github`
- **authoring/meta skills** like `skill-creator`
- **orchestration/runtime layers** like `prose`

That separation would help IEF too.

## Recommended next step after this note

The next logical follow-up is to answer:

> Is there a shared or emerging standard for how IDE/CLI agents load `SKILL.md`-style capability packs?

That matters because:
- `skill-creator` and OpenClaw explicitly reference AgentSkills-compatible conventions
- IEF already has its own `skills/` folder
- the strategic question is no longer only “which skills,” but also “which skill-loading contract is safe to align with?”
