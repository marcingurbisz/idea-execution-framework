# Always-on memory agent review for IEF

## TODO
* Work on feedback and questions below

## Feedback and questions
* Do they have example prompts for the consolidation? 
* Why SQLLite and not row text files? SQLLite keeps structured text or something more?
* I wonder if for IFE better would be periodic summarization or consolidation. Or maybe both can be used in different cases. By consolidation I mean keeping old memories untouched instead replacing them as in summarization.
* I think I will go for explicit task/human triggered consolidation/summarization for now e.g. item on todo list marked as periodic (Mode C)
* Should consolidation/summarization work on diff from previous consolidation or on whole repo. I think both should be the input.
* Is there something similar (consolidation or summarization) in openclaw?
* Better name for the whole topic than Always on memory agent?
* In general I agree with your conclusions and ideas for moving forward.
* I wonder what to do with this file. I think it requires more of a summarization then consolidation

## Scope

Reviewed the public example at `GoogleCloudPlatform/generative-ai/gemini/agents/always-on-memory-agent`.

Question for IEF:

- what is the valuable pattern here?
- how could it be adapted to IEF?
- can the idea work without API keys?
- how is it different from normal chat summarization?

## What the example actually is

The Google example is not just “chat with memory.” It is a small memory system with four distinct pieces:

1. **continuous ingestion**
   - watches an inbox folder and also accepts HTTP ingestion
   - supports text, images, audio, video, and PDFs
   - extracts structured memory fields such as summary, entities, topics, importance, and source
2. **persistent storage**
   - keeps memories in SQLite
   - stores both raw text and processed memory fields
   - tracks whether a memory was already consolidated
3. **periodic consolidation**
   - every 30 minutes by default, it revisits unconsolidated memories
   - finds connections across them
   - writes higher-level summaries and insights
4. **query layer**
   - answers questions from the stored memories plus consolidation history
   - cites the underlying memory items

Conceptually, it is closer to a tiny background knowledge refinery than to ordinary chat memory.

## Architecture pattern worth stealing

The most reusable part for IEF is the **three-loop model**:

### 1. Capture loop

New information enters a durable inbox instead of living only in chat scrollback.

Examples:

- raw notes
- copied URLs
- screenshots
- meeting transcripts
- “remember this” task fragments

### 2. Consolidation loop

The system periodically reprocesses recent items and produces higher-level artifacts:

- merged summaries
- patterns
- links between items
- “what changed?” notes
- “what matters now?” notes

### 3. Query loop

Later work does not depend only on the latest summary. It can query the accumulated memory layer.

This fits IEF very well because IEF already treats the repo as a durable shared blackboard.

## What IEF should borrow

IEF does **not** need to copy the Google stack itself.

It should borrow these ideas instead:

### Durable inbox

IEF currently stores decisions and plans well, but it has no first-class “unprocessed input queue.”

Useful addition:

- `memory/inbox/` for raw captures

Examples of contents:

- `*.md` research notes
- copied problem statements
- pasted external model outputs
- screenshots or PDFs to be interpreted later

### Structured memory cards

Instead of leaving everything as raw text, IEF could standardize extracted memory artifacts.

Example fields:

- source
- raw input reference
- short summary
- entities / systems / repos
- themes / topics
- importance
- open questions
- related items

### Consolidation artifacts

This is the strongest idea in the example.

IEF already has TODO and LOG, but it could also keep periodic consolidation artifacts such as:

- weekly insight notes
- cross-project pattern notes
- merged research summaries
- dependency / risk digests
- “things worth escalating” notes

Good candidate layout:

- `memory/consolidations/`

### Queryable repo memory

The example separates “memory creation” from “answering questions over memory.”

IEF can do the same even without a dedicated app server:

- ask an agent to answer from `README.md`, `LOG.md`, `TODO.md`, research notes, and consolidation notes
- require the answer to cite repo artifacts, not only improvise from local context

## Recommended IEF adaptation

### Smallest useful version

Add a lightweight memory-layer convention inside IEF:

1. `memory/inbox/` — raw unprocessed captures
2. `memory/cards/` — extracted durable memory items
3. `memory/consolidations/` — periodic higher-level syntheses
4. a skill that teaches agents when to ingest vs consolidate vs query

This would make IEF better at handling long-running research and multi-session projects.

### Operational workflow

#### Ingestion

An agent periodically checks `memory/inbox/` and turns raw items into structured memory cards.

#### Consolidation

On a schedule or after enough new items accumulate, an agent reviews recent cards and writes:

- clusters
- repeated themes
- contradictions
- next-step suggestions

#### Query

When a human asks “what do we know?” or “what matters now?”, the agent uses:

- current project README
- TODO / LOG
- structured cards
- consolidation artifacts

That is more robust than answering from the current chat alone.

## Can this work without API keys?

## Short answer

**The exact Google implementation: no.**

It explicitly expects a Gemini API key and is built around programmatic model calls.

**The underlying idea: yes, partially.**

IEF can reuse the pattern without API keys, but the no-key version is less autonomous and less reliable.

## Practical modes

### Mode A — true always-on service with API key

This is the cleanest version.

- background process ingests automatically
- scheduled consolidation runs unattended
- query API is always available
- best fit for 24/7 memory maintenance

### Mode B — browser-first / subscription-first memory maintenance without API key

This can work if a signed-in browser agent or IDE agent periodically does the work and writes artifacts back into the repo.

Examples:

- Oracle-style browser escalation processes inbox items
- a scheduled agent loop ingests raw captures into repo memory
- a human-triggered nightly “consolidate memory” run updates the artifacts

What works well:

- ingestion and consolidation as explicit tasks
- durable repo artifacts
- lower direct API spend if the user already has a subscription plan

What does **not** work as well:

- unattended 24/7 operation
- reliable background processing on a headless machine
- clean machine-to-machine integration

### Mode C — human-assisted consolidation without API key

This is the cheapest and simplest path.

- raw inputs accumulate in repo folders
- the agent is asked periodically to ingest and consolidate them
- results are committed into repo memory

This is not truly “always on,” but it preserves the important idea: memory is continuously refined over time, not only summarized once.

## Bottom line on API keys

Without API keys, IEF can absolutely adopt **ingestion + consolidation as a workflow**.

Without API keys, IEF probably should **not** aim first for a fully unattended daemon like the Google example.

For IEF, the most realistic progression is:

1. human-triggered or scheduled repo-based ingestion/consolidation
2. browser-assisted automation on a signed-in machine
3. full unattended service only when API-mode is worth the cost

## How this differs from normal chat summarization

This is the most important distinction.

| Ordinary summarization | Always-on memory pattern |
|---|---|
| compresses one conversation | accumulates knowledge across many inputs |
| usually overwrites detail | preserves raw items plus processed artifacts |
| mostly linear | adds cross-links between separate items |
| triggered by context pressure | triggered as an intentional maintenance loop |
| summary is often ephemeral | memory artifacts are durable and queryable |
| weak provenance | can cite concrete memory items and consolidations |
| little or no background processing | explicit ingestion and consolidation stages |

In plain language:

- summarization says “here is a shorter version of what just happened”
- memory consolidation says “here is what these many inputs mean together, what connects them, and what should influence future work”

Most chat products do the first.

The interesting part of the Google example is that it tries to do the second.

## Why this matters for IEF

IEF already solves part of the persistence problem by keeping repo artifacts.

The gap is that repo memory today is mostly:

- manually curated
- document-oriented
- project-level

The always-on memory idea adds a missing middle layer:

- raw capture
- structured extraction
- periodic consolidation
- later query over the refined memory set

That would make IEF better for:

- longer research threads
- multi-project workspaces
- repeated context switching
- nightly or weekly agent maintenance loops

## Recommended next steps for IEF

1. add a lightweight `memory/inbox/` convention
2. define a memory-card template for extracted items
3. define a consolidation-note template
4. add a reusable skill for “ingest / consolidate / answer from memory”
5. only later decide whether a wrapper or daemon is justified

## Plain-language conclusion

The Google example is valuable for IEF mainly as a **workflow pattern**, not as a dependency choice.

The best reusable idea is:

- keep an inbox of raw material
- transform it into structured memory
- periodically consolidate it into higher-level insights
- answer later from the durable memory layer instead of from chat alone

That idea works in IEF even without API keys.

What does not transfer cleanly without API keys is the **fully autonomous always-on service**.

For IEF, the right first move is a repo-native memory workflow, not a daemon.