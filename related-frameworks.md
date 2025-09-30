
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

Where this framework fits
- Bias to simplicity: git repo is the blackboard, plan, and memory; agents publish decisions and next steps into files.
- Tools are optional: we can compose with any of the above later (e.g., use LangGraph for durability) without changing the repo contract.
- Start collaborative, then automate: prioritize L1–human clarity; adopt multi-agent orchestration only when it clearly reduces human load.

What we borrow / avoid
- Borrow: role clarity and delegation (MetaGPT, CrewAI), explicit decision records (LangGraph eval/observability mindset), lightweight handoffs (Swarm/Agents SDK).
- Avoid (for now): heavy runtime/state infra, proprietary control planes, and lock-in to specific orchestration frameworks. Git remains canonical state.