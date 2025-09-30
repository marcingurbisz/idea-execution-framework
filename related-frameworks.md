
## Related frameworks (research)
A quick map of nearby ecosystems and how they differ from this README-first, git-as-ledger approach:

- MetaGPT – "software company in a box": opinionated roles and SOPs for multi-agent software creation. Strong for end-to-end scaffolding; heavier than we need. https://github.com/FoundationAgents/MetaGPT
- AutoGen – conversational multi-agent framework: flexible agent chats and tool calls; requires you to encode process/state. We keep process in the repo. https://microsoft.github.io/autogen/
- CrewAI – Crews (autonomy) + Flows (event-driven control): fast, standalone Python framework; great for orchestration, but introduces runtime/state infra beyond git. https://github.com/crewAIInc/crewAI
- LangGraph – low-level, stateful agent/workflow graphs: durable execution, HITL, memory, deployment. Ideal for production agents; our pilot keeps minimal infra, with git as state. https://langchain-ai.github.io/langgraph/
- OpenAI Swarm / Agents SDK – lightweight handoffs and functions; now superseded by Agents SDK for prod. Useful patterns, but externalizes state. https://github.com/openai/swarm
- AutoGPT (Platform) – build/deploy/manage continuous agents with UI, blocks, marketplace. Powerful, platform-centric; beyond our lightweight goals. https://github.com/Significant-Gravitas/AutoGPT
- OpenHands (ex-OpenDevin) – full developer agent platform (terminal, files, browser). Great for coding automation; heavier runtime than our README-first pilot. https://github.com/All-Hands-AI/OpenHands
- GPT-Engineer – prompt-to-code CLI; simple, repo-oriented but single-agent. We share the repo-first ethos without the CLI lock-in. https://github.com/AntonOsika/gpt-engineer
- GitHub Spec Kit – spec authoring templates and review rituals for feature planning. Valuable for richer design docs, but focuses on pre-execution specs rather than repo-as-blackboard execution. https://github.com/github/spec-kit
- BabyAGI – task list manager (execute/create/prioritize) with simple loop memory and optional vector recall; highlights task-centric planning. https://github.com/yoheinakajima/babyagi
- ChatDev – staged, role-based SDLC pipeline where artifacts flow stage-to-stage; artifacts act as durable memory. https://github.com/OpenBMB/ChatDev

---

## Memory approaches (quick comparison)

Most frameworks pair a short-term conversational or scratchpad buffer with either semantic memory (vector retrieval) and/or artifacts-as-memory (files, docs, code) that persist beyond a single run. We bias to the latter.

| Framework | Memory (short-term) | Memory (long-term / persistent) | Artifacts-as-Memory | Notes |
|---|---|---|---|---|
| AutoGPT | Loop buffer / scratchpad during Thought→Action→Reflect | Optional vector store (embeddings) | Files on disk / repo snapshots | Early autonomous agent; simple curated persistence often beats heavy stacks for small scopes. |
| BabyAGI | Current task context & dynamic task list | Vector DB for prior results/notes | Task outputs and notes | Clear separation: execute, create, prioritize; recall depends on chunking/indexing quality. |
| LangChain Agents / LangGraph | Buffer/summary memories via memory classes | Pluggable vector retrievers (FAISS, Pinecone, etc.) | Tool I/O and generated files | Modular: ReAct agents or multi-agent graphs; durable execution in LangGraph. |
| CrewAI | Per-agent context buffers (role-scoped) | Optional per-role retrieval; shared artifacts across agents | Plans, docs, PRs | Emphasizes team boundaries and observability. |
| Microsoft AutoGen | Dialogue history between agents/humans | External stores (DB/files) optionally referenced | Code, logs, test outputs | Chat-centric multi-agent; artifacts/logs carry context forward. |
| MetaGPT | Per-step context governed by SOPs | Project artifacts as persistent knowledge; optional retrieval | Requirements, designs, code in repo | Role/SOP-driven “virtual company”; repo as blackboard. |
| ChatDev | Stage-local buffers (design→code→test→doc) | Outputs from earlier stages persist to later ones | Design docs, code, reports | Waterfall-like pipeline; artifacts are primary long-term memory. |

> Takeaway: Start simple with repo artifacts and short-term buffers; add semantic retrieval only when the repo+context window isn’t enough.

Where this framework fits
- Bias to simplicity: git repo is the blackboard, plan, and memory; agents publish decisions and next steps into files.
- Tools are optional: we can compose with any of the above later (e.g., use LangGraph for durability) without changing the repo contract.
- Start collaborative, then automate: prioritize L1–human clarity; adopt multi-agent orchestration only when it clearly reduces human load.

## Learnings
- Borrow: role clarity and delegation (MetaGPT, CrewAI), explicit decision records (LangGraph eval/observability mindset), and lightweight handoffs (Swarm/Agents SDK).
- Avoid (for now): heavy runtime/state infra, proprietary control planes, and lock-in to specific orchestration frameworks. Git remains canonical state.
- For small/medium scopes, curated repo artifacts plus long-context models suffice; add retrieval only when repeated lookup pain or context-window pressure appears.