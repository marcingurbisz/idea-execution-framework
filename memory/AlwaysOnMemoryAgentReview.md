# Repo memory refinement review for IEF

This is a summarized follow-up to the earlier review of `GoogleCloudPlatform/generative-ai/gemini/agents/always-on-memory-agent`.

For IEF, the better umbrella term is **repo memory refinement loop** rather than “always-on memory agent” because the useful pattern is not the daemon itself. The useful pattern is:

1. capture raw inputs durably
2. refine them into structured memory
3. consolidate them into higher-level notes
4. answer future questions from that memory layer

## Short answers to the follow-up questions

### Do they have example prompts for consolidation?

Yes. The Google example keeps the runtime trigger prompt very small:

> `Consolidate unconsolidated memories. Find connections and patterns.`

The real behavior comes from the consolidation agent instruction, which says to:

1. read unconsolidated memories
2. stop if there are fewer than two
3. find connections and patterns
4. create a synthesized summary and one key insight
5. store `source_ids`, `summary`, `insight`, and `connections`

For IEF, explicit prompt templates would be more useful than copying that prompt literally.

Suggested IEF prompts:

#### Consolidation prompt

"Read the recent repo memory inputs and produce a new consolidation note. Preserve source artifacts. Identify recurring themes, contradictions, important changes, open questions, and recommended next actions. Cite the source files you used."

#### Summarization prompt

"Summarize this single artifact or work cycle for handoff. Do not merge or overwrite older memory. Capture decisions, progress, blockers, and immediate next actions. Cite the source files you used."

#### Query prompt

"Answer from repo memory only. Use `README.md`, `LOG.md`, `TODO.md`, relevant memory cards, and recent consolidations. Cite the files that support the answer."

### Why SQLite instead of raw text files?

In the Google example, SQLite is used because the system needs frequent reads and updates against structured records:

- raw text
- summary
- entities
- topics
- importance
- source
- created timestamp
- `consolidated` flag
- connections to other memory items
- separate consolidation history
- processed-file tracking

So SQLite is mostly acting as a small mutable state store with a simple schema.

It is not storing something fundamentally richer than text. Much of the structure is still text or JSON serialized into text columns. The value is convenient updates and queries.

For IEF, the better starting point is different:

- keep Markdown/text files as the canonical source of truth
- keep them reviewable and git-friendly
- add a derived index later only if retrieval or bookkeeping becomes painful

So the answer is:

- **Google example:** SQLite makes sense
- **IEF v1:** raw repo files should remain primary
- **IEF later:** optional derived SQLite/index layer could help, but should not replace repo memory files

### Summarization or consolidation?

IEF should use **both**, but for different jobs.

#### Use summarization when:

- compressing one session, file, or artifact
- writing a handoff
- reducing reading cost for humans
- preserving the main thread of a single work cycle

#### Use consolidation when:

- combining multiple inputs without deleting them
- finding cross-cutting patterns
- comparing notes over time
- surfacing contradictions and repeated themes
- producing “what matters now?” or “what changed?” notes

Recommended default for IEF:

- **summarization** for handoffs and single-thread compression
- **consolidation** for durable memory refinement

### Is explicit human/task-triggered Mode C a good first step?

Yes. This is the right first implementation for IEF.

Mode C keeps the pattern while avoiding premature daemon complexity:

- a TODO item or human request triggers the run
- the agent ingests and/or consolidates repo memory
- the result is written back into the repo
- the work is committed like any other IEF loop output

That fits IEF better than a hidden background service.

### Should the input be the diff or the whole repo?

It should be **delta plus stable context**, not just one or the other.

Recommended input set for an IEF consolidation run:

1. items added or changed since the last consolidation
2. current `README.md`, `LOG.md`, and `TODO.md`
3. the most recent one or two consolidation notes
4. any specifically relevant long-lived memory files

So:

- **diff only** is too narrow
- **whole repo every time** is too expensive and noisy
- **delta + selected stable context** is the best default

### Is there something similar in OpenClaw?

Yes, in spirit, but not in the same form.

OpenClaw already has:

- durable Markdown memory files such as `memory/YYYY-MM-DD.md` and `MEMORY.md`
- a silent **pre-compaction memory flush** that reminds the model to write durable notes before auto-compaction
- session compaction that persists summaries into session transcripts
- separate research notes exploring a richer offline memory system with Markdown as canonical storage plus a derived index and reflection/recall tools

So OpenClaw is similar in that it treats memory as something durable and actively maintained.

It is different in that:

- it is more workspace- and session-centric
- it prefers Markdown as source of truth
- it couples memory maintenance to session/compaction flow rather than to a separate inbox-plus-consolidation daemon

The closest IEF takeaway is: **repo-native memory plus explicit reflection beats chat-only memory**.

### Better name than “always-on memory agent”?

Recommended IEF term: **repo memory refinement loop**.

Why this name:

- “repo” matches IEF’s source of truth
- “memory” keeps the core idea
- “refinement” includes both summarization and consolidation
- “loop” fits IEF’s operational style better than “agent” or “daemon”

### What should happen to this file?

It should be summarized, not endlessly extended.

That is what this version does: it keeps the core conclusions, answers the follow-up questions, and drops the unresolved TODO header.

## What is valuable in the Google example?

The most reusable part is the **capture → consolidate → query** pattern.

The example is not valuable because it uses Gemini, ADK, or SQLite specifically.
It is valuable because it separates three concerns that many agent workflows blur together:

1. **capture** — new inputs enter a durable store
2. **refine** — inputs are turned into structured memory
3. **query** — later questions are answered from accumulated memory, not only the current chat

Conceptually, it is closer to a small background knowledge refinery than to ordinary chat memory.

## Recommended IEF adaptation

### Smallest useful version

Add a lightweight repo-native memory refinement layer:

1. `memory/inbox/` — raw captures
2. `memory/cards/` — extracted memory items
3. `memory/consolidations/` — periodic synthesis notes
4. a reusable skill for when to ingest, summarize, consolidate, or answer from memory

### Recommended workflow

#### Capture

Put raw material into `memory/inbox/`:

- rough research notes
- copied problem statements
- pasted model outputs
- screenshots or PDFs to interpret later

#### Refine

Turn raw items into cards with fields such as:

- source
- short summary
- systems/repos/entities involved
- themes
- importance
- open questions
- related items

#### Consolidate

When enough change has accumulated, create a note that records:

- repeated themes
- contradictions
- cross-project connections
- risks and escalation candidates
- “what matters now?”

#### Query

When asked later, answer from:

- `README.md`
- `LOG.md`
- `TODO.md`
- memory cards
- consolidation notes

and require citations to repo artifacts.

## Can this work without API keys?

### Short answer

- **The exact Google implementation:** no
- **The underlying IEF pattern:** yes

The Google example assumes programmatic model access.

IEF can still adopt the workflow without API keys through browser-based or human-triggered runs, but it will be less autonomous.

### Practical modes

#### Mode A — unattended service with API key

Best for real always-on maintenance.

#### Mode B — browser-first maintenance on a signed-in machine

Works for stronger automation without committing to API-first operation.

#### Mode C — explicit human/task-triggered refinement

Best first step for IEF.

Recommended progression:

1. Mode C first
2. browser-assisted Mode B if needed
3. true service/daemon only when clearly justified

## Difference from normal chat summarization

| Ordinary summarization | Repo memory refinement |
|---|---|
| compresses one conversation | accumulates knowledge across many inputs |
| often replaces detail | preserves sources and adds higher-level notes |
| mostly linear | cross-links separate artifacts |
| usually triggered by context pressure | triggered as deliberate maintenance |
| often ephemeral | durable and queryable in the repo |
| weak provenance | can cite concrete artifacts |

In plain language:

- summarization says: “here is a shorter version of what just happened”
- consolidation/refinement says: “here is what these many inputs mean together and what should shape future work”

## Final recommendation for IEF

Do not build a daemon first.

Start with a **repo memory refinement loop**:

1. add `memory/inbox/`
2. define a memory-card template
3. define a consolidation-note template
4. add a skill for ingest/summarize/consolidate/query behavior
5. run it explicitly from TODO items or human requests

That keeps the good part of the always-on memory idea while staying aligned with IEF’s repo-native workflow.