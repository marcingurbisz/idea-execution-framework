# Oracle integration roadmap for IEF

## Goal

Turn the earlier Oracle research into a concrete path that makes Oracle-style escalation usable inside IEF without jumping straight into a large standalone product build.

## Design stance

Build the integration in layers:

1. **teach the workflow first**
2. **standardize repo artifacts second**
3. **automate browser-first execution third**
4. **add API-mode and broader provider support only where it clearly pays off**

This keeps IEF aligned with its repo-first philosophy and avoids overbuilding.

## Constraints

- IEF is currently a framework/docs repo, not a mature runtime tool
- the cheapest path for many users is still Oracle browser mode with ChatGPT Plus
- API mode is useful, but should be treated as an explicit premium/reliability option
- the integration should produce durable repo artifacts, not only hidden local state

## Target user journeys

### Journey 1 — manual escalation

An agent gets stuck, prepares a prompt bundle, and the human or agent runs Oracle manually.

Success condition:

- the request and result are captured in repo memory
- the escalation is reproducible
- the next agent can continue from the stored artifact

### Journey 2 — skill-guided escalation

The agent knows that Oracle exists, when to use it, how to prepare context, and where to store outputs.

Success condition:

- Oracle is part of the agent's working vocabulary
- the agent does not need ad hoc prompt instructions every time

### Journey 3 — lightweight automated escalation

A repo-local wrapper or script runs Oracle with a standard artifact layout and status tracking.

Success condition:

- one command creates a session folder and stores request/response/meta
- the workflow stays browser-first by default

## Recommended phases

## Phase 0 — foundation in the IEF repo

Purpose: make Oracle integration visible, teachable, and repo-native before building runtime automation.

Deliverables:

- a concrete roadmap document
- a `skills/` convention for IEF
- an Oracle skill that teaches when and how to escalate
- documented artifact layout for Oracle session outputs

Why first:

This gives immediate value even before any custom tooling exists.

## Phase 1 — repo-local workflow standardization

Purpose: make Oracle usage consistent across IEF projects.

Deliverables:

- canonical session folder layout, e.g. `memory/oracle-sessions/<id>/`
- request template
- response/meta expectations
- log/TODO conventions for Oracle escalations
- README guidance on when to choose browser mode vs API mode

Acceptance criteria:

- an IEF project can use Oracle consistently without custom chat instructions
- session artifacts are easy to inspect in Git

## Phase 2 — thin wrapper / helper

Purpose: reduce manual steps while keeping the integration small.

Deliverables:

- a thin `ief-oracle` command or script spec
- browser-first default behavior
- optional API-mode override
- deterministic output paths
- dry-run / preview support

Acceptance criteria:

- one command can create a request bundle and invoke Oracle
- outputs land in repo-local memory automatically

## Phase 3 — durable session handling

Purpose: make long-running Oracle usage reliable enough for repeated agent workflows.

Deliverables:

- status inspection conventions
- reattach/resume conventions
- failure/retry handling
- guidance for remote signed-in browser hosts

Acceptance criteria:

- long browser runs are not "fire and forget"
- the next loop iteration can pick up a session cleanly

## Phase 4 — broader provider support

Purpose: expand beyond the first ChatGPT-centric path only after the workflow is stable.

Deliverables:

- explicit API-mode guidance
- optional Claude/Gemini usage notes
- selection heuristics by cost/reliability/task type

Acceptance criteria:

- the default path remains simple
- premium paths are documented as intentional trade-offs

## Immediate next steps

These are the steps worth executing now inside the IEF repo.

### Step 1 — add a `skills/` convention to IEF

Why:

- this is already strongly supported by the OpenClaw review
- it gives IEF a place for reusable capability instructions
- Oracle integration becomes a concrete skill instead of a vague idea in memory notes

### Step 2 — add an Oracle consultation skill

Why:

- agents need explicit reusable guidance for when Oracle should be used
- the skill can define artifact layout and decision rules immediately
- it works even before a wrapper exists

### Step 3 — document the Oracle session artifact contract

Why:

- IEF's main advantage is durable repo memory
- Oracle integration should standardize request/response/meta file locations early

### Step 4 — update README guidance

Why:

- the repo should explain that `skills/` is now part of the practical workflow
- Oracle becomes discoverable from the main entry point, not only buried in memory notes

## What to defer

These are valuable, but should wait until the above exists.

- building a full standalone Oracle clone inside IEF
- full Claude browser automation support
- automatic skill registry/distribution
- complex secret injection and per-skill config systems
- silent auto-escalation without clear human/operator awareness

## Recommended execution order

1. roadmap document
2. `skills/` convention
3. Oracle skill
4. README update
5. optional thin wrapper spec

## What execution means in this repo

For the current IEF repo, "execute the next Oracle integration steps" should mean:

- create the missing skill structure
- encode Oracle usage as a reusable skill
- document the session artifact shape
- make the integration discoverable from the README

That is the smallest useful real implementation here.

## Plain-language conclusion

IEF should not start by re-implementing all of Oracle.

It should start by making Oracle a **first-class repo workflow**:

- a documented escalation pattern
- a reusable skill
- a standard artifact layout
- a browser-first recommendation with API-mode as an explicit paid option
