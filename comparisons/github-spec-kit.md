# Comparison: Idea Execution Framework vs. GitHub Spec Kit

## Overview of GitHub Spec Kit
- GitHub's Spec Kit is a documentation-first toolkit for crafting product and engineering specifications.
- It centers on markdown templates (e.g., PR/issue templates and a `spec-template.md`) that walk teams through background, problem, goals, non-goals, proposed solution, metrics, and rollout considerations.
- The repository provides guidance on spec review workflows inside GitHub, encouraging collaborative iteration through pull requests before implementation begins.
- Spec Kit presumes work happens in GitHub, but focuses on planning artifacts rather than storing execution state.

## Overlapping Intentions
- Both frameworks emphasize clarity before execution: they capture context, requirements, and rationale in human-readable markdown.
- Each encourages collaborative workflows inside GitHub with version-controlled artifacts so decisions are auditable.
- Both default to lightweight tooling—markdown files, issues, and pull requests—rather than heavyweight orchestration platforms.

## Key Differences
- **Scope of lifecycle**: Spec Kit concentrates on pre-implementation planning (writing and reviewing specs). The Idea Execution Framework spans the full cycle—from clarifying the idea to planning, execution, and capturing learnings in the repo.
- **Primary artifacts**: Spec Kit revolves around a single spec document shepherded through review. The Idea Execution Framework treats the entire repository as the "blackboard," mixing specs, plans, decision logs, learnings, and produced assets.
- **Role model**: Spec Kit presumes a human team collaborating directly. The Idea Execution Framework introduces an explicit Lead Agent (L1) role coordinating with the human and optionally delegating to specialized agents.
- **Memory & state**: Spec Kit expects project memory to live in the spec and GitHub discussions. The Idea Execution Framework formalizes repo-based memory (decision notes, learnings, TODOs) to support autonomous agent execution without external state stores.
- **Automation posture**: Spec Kit is human-centric and doc-driven. The Idea Execution Framework is automation-friendly, aiming to minimize human involvement over time while keeping the repo transparent.

## Complementary Usage
- Use Spec Kit-style documents during Phase A/B of the Idea Execution Framework when a richer product spec is required; store the spec inside the repo alongside decision notes.
- Combine Spec Kit's review rituals with the Idea Execution Framework's continuous execution logs to maintain rigor without losing velocity.
- Treat Spec Kit templates as optional plug-ins: import them when stakeholders need formal approval processes, but keep the execution framework lightweight for day-to-day agent work.
