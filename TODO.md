## AGENTS.md minor improvements
* Do keeping "Agent Instructions (Shared)" header with text "This file is intended to be shared across projects that use IEF." gives some benefit. What do you think about skipping this header and starting AGENTS.md directly with "Idea Execution Framework (IEF)" ?
* Add to AGENTS.md that TODO items once done may be reworked into log term documentation under README.md and/or docs. For now it is mainly human role to make sure that things done in scope of given todo are later properly incorporated into docs/long term memory (usually by creating proper todo items for that) - or maybe this more belongs to README.md? By saying "mainly human role" I do not want to hold back Agent from doing it.

## [FOR HUMAN] Prepare todo items
* Define next TODO items for
  * skills
  * oracle skill
  * AlwaysOnMemoryAgentReview
  * always-on-agents-lab
* Hermes Agent research
* Test Opus 4.6
* Research https://www.infoq.com/articles/enterprise-spec-driven-development/
* Should I move TODO.md under docs? For now I'd rather not do it
* What do I need to fully delegate project execution (I'm only PO without looking at code)
  * depend on project for sure but let's focus on flowlite
  * what agents still need to have to be able continuously run the loops, generate new todo items, respond to events (prs, bugs, incidents on test env)
  * Agent activities:
    * writing reports for me
  * Make a test that will let the Agent free? :)
  * What feedback from me is still crucial?

## [DONE 2026-03-15] Work on feedback for Oracle
* "Reuse the architecture, not the whole product surface" - why not use steipete/oracle as it is? Or other option ife-oracle calls steipete/oracle? What are pros and cons of each 3 approaches? Wait I see in SKILL.md that actually steipete/oracle is in use so you've changed the mind and decided to use steipete/oracle?
* If there is no provider for Claude.ai browser approach then let's keep it out of scope for now. Probably I will never got into it. More probable is is that we will switch to API calls.
* AnAnswer to open questions from md file:
1. Should IEF keep oracle sessions per repo or in one shared workspace location? - for now we can go for approach that is simplier
2. Should the main agent write escalation requests automatically, or only when explicitly prompted? - should be autonomous, can decide on its own
3. Is Claude Pro a required provider for v1, or is ChatGPT Plus enough for the first usable version? - let's de-scope Claude Pro
4. Do you want the eventual implementation to live inside this repo as a reference spec, or inside a separate tool repo? - not sure yet. Let's do what's simpler for now.
* Summarize [text](memory/OracleBrowserAutomationPlan.md) and [text](memory/OracleIntegrationRoadmap.md) into one concise document
* SKILL.md
  * I think that in it's worth to add that oracle/external models has better tools for web research for the tasks that quality of the research is important agent should use oracle. Do you agree?
  * Remove parts about the costs. I do not want you to consider it for now. Browser is now default and if this will not work reliably we will switch to API key
  * I have removed OracleApiCostNotes.md
* Oracle request template and session layout comes from steipete/oracle or this is our idea?
* How Oracle with API key works. Were do you put API key? Is it safe and not available to agent?

## [DONE 2026-03-15] Work on TODO in idea-execution-framework/memory/AlwaysOnMemoryAgentReview.md

## [DONE 2026-03-15] How OpenClaw instance could help/extend IFE?
Do research and save the results in md file. Can OpenClaw run on ChatGPT Plus or Claude.ai Pro subscription?

## [DONE 2026-03-15] Use oracle skill to search for openclaw concrete skills that can be beneficial for IEF

## [DONE 2026-03-15] Is there some standard for loading SKILLS by IDE/CLI agents? Maybe this is a good task for Oracle?

## [DONE 2026-03-09] Check Oracle API cost vs ChatGPT Plus
research what additional cost API-mode Oracle introduces beyond a ChatGPT Plus subscription and document the practical cost trade-offs.

## [DONE 2026-03-09] Plan next Oracle integration steps
turn the existing Oracle research into a concrete implementation roadmap for IEF.

## [DONE 2026-03-09] Execute first Oracle integration steps
apply the roadmap in-repo, starting with the smallest useful integration artifacts.

## [DONE 2026-03-09] Check https://github.com/GoogleCloudPlatform/generative-ai/tree/main/gemini/agents/always-on-memory-agent
How we could use it in IEF? Can it or the idea be used without API keys but with agent doing Ingestion and Consolidation? How this is different from summarization process that most chats and agent platforms do?

## [DONE 2026-03-09] New project
Create a new project under workspace for finding best setup for having agents always on. E.g. I can run graphical session from my laptop "localy" where I have my agents running. But I want also be able to connect to it from other computer when I'm not at home. Best it would be if I can connect using web browser without installing anything. I tried RustDesk from webbroser but it was a little bit lagish.
How Peter Steinberg setup looks like in this respect?
Similar setup with linux?
BTW: Can I move GitHub Copilot sessions from one computer to another? What about codex and claude code.
