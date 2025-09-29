# Idea Execution Framework

## Vision
Make executing ideas with an AI efficient and transparent, with plans, decisions, artifacts, and learnings versioned and human‑auditable. The Lead Agent collaborates with the human to clarify the idea and drive execution in observable steps. Human involvement is minimized by phase: high during early idea elaboration, lower during planning/prototyping, and as low as possible during execution within agreed scope. Specialized agents may be involved when helpful, but are not required for small projects.

## Core Principles
- Git repo as shared project state/blackboard.
- Persist all plans, decisions, artifacts, and learnings in the repo.
- Human provides the vision and makes high-level decisions; the Lead Agent (level-1) collaborates and may delegate when beneficial.
- Memory is in repo: both working artifacts and distilled interaction learnings live as files here.
- Repo is the single source of truth and synchronization medium; plans and specs live here and drive delegation.

## Operating Model
Roles
- Human: sets idea, constraints, core principles, requirements, approves major decisions, provides clarifications
- Lead Agent (L1): a critical partner who helps think through and clarify the idea, expands high-level ideas into actionable detail, turns ideas into plans, executes tasks, and keeps the repo in sync; may involve specialized agents if needed.
- Specialized Agents (L2+, optional): coding, ops, research, etc.

## Iteration rhythm
- Updates to the repo happen whenever useful (often after meaningful exchanges) and at least once per focused work cycle to keep the repo the single source of truth.
- Phases are flexible and can overlap:
	- Phase A – Idea Elaboration (high human involvement): Human states goal/idea; L1 probes, challenges, and expands; fast back-and-forth in README; capture assumptions and decisions.
    - Phase B – Planning & Prototyping (moderate human involvement): L1 drafts approaches, proposes tasks, executes small spikes; human reviews key choices.
    - Phase C – Execution (low human involvement): L1 works autonomously within agreed scope; updates repo regularly; escalates only for major decisions or boundary changes.

## Repository Structure
Start README-first. Add folders only when the project grows; keep the repo the single source of truth.

- README.md
	- vision
	- core principles
	- operating model
	- repository structure
	- interaction memory
	- iteration rhythm
	- TODOs (Lead Agent, Human)
	- roadmap
	- decision notes (template)
	- learnings (template)
- related-frameworks.md (research on similar approaches)
- spec-kit-comparison.md (detailed comparison with GitHub Spec Kit)

## Interaction Memory
Interaction memory captures distilled insights from conversations and actions so future steps improve. It is part of the repo memory and should be lightweight and human-auditable.
- Minimal fields:
	- Date/Context
	- Insight (what we learned or decided and why)
	- Outcome (what changed)
- Keep next steps in the TODO sections, not here. Start inline in README under a “Learnings” subsection; extract to `interaction-log.md` only when it grows.

## TODO – Lead Agent
- ~~Deep research: other frameworks similar to the one that is described here~~ ✅ See `related-frameworks.md` and `spec-kit-comparison.md`
- Research memory approaches (Claude Flow and others) and propose a simple initial method here (maybe alternative to our approach with "Decision Notes" and "Learnings" in readme)
- Consider incorporating lightweight constitutional principles based on Spec Kit analysis
- Evaluate structured clarification workflows for Phase A (Idea Elaboration)

## TODO – Human
- Define/refine the vision iteratively, perform researches with the Lead Agent (using Copilot as L1 for now).
- Research with AI https://github.com/ruvnet/claude-flow
    - Clarify the difference between the "neural module" and "Memory system" in Claude Flow.
- Review how multi-agent features work in Codex/Claude CLI; identify patterns to reuse.
- Check research chat: "Badania nad AI i pamięcią".
- Think what should be the structure of the projects that will be used to execute ideas according to the Idea Execution Framework

## Roadmap
- Phase 1: Execute Phase A for "Idea Execution Framework" (this repo is the pilot) till Phase B or till Phase 2 below.
- Phase 2: When the framework feels solid, start a second project executed according to it.

---

## Decision Notes (template)
- Date: YYYY-MM-DD
- Context:
- Decision:
- Alternatives considered:
- Rationale:

## Learnings (template)
- Date/Context:
- Insight:
- Outcome:

### 2025-01-03 - GitHub Spec Kit Comparison Analysis
**Context**: Analyzed GitHub Spec Kit's Specification-Driven Development approach to compare with our Idea Execution Framework.

**Insight**: GitHub Spec Kit provides complementary rather than competing methodology. Key differences:
- **Formality**: Spec Kit uses formal templates and constitutional governance vs our lightweight README-first approach
- **Structure**: Spec Kit has rigid phase gates and validation checkpoints vs our flexible overlapping phases  
- **Governance**: Spec Kit enforces immutable constitutional principles vs our guidelines-based approach
- **Tooling**: Spec Kit provides comprehensive CLI and template ecosystem vs our minimal tooling requirements

Both share core values: repo-centricity, human-AI collaboration, iterative refinement, and documentation as code.

**Outcome**: Created comprehensive comparison analysis in `spec-kit-comparison.md`. Added Spec Kit to `related-frameworks.md`. Identified potential hybrid approaches and learning opportunities for both simple and complex projects.