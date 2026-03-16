## [DONE 2026-03-16] Review last commit
* especially review how I allow for not having TODO.md but having all todo items under todo/<topic>.md files. In such case loop will usually be executed for given topic file and not whole repo. But one can also ask for execution of all todos over all topics. What do you think about this change?
* improvements if you find something worth changing

### Log
- 2026-03-16 - Review finding: the previous docs version was internally inconsistent because it described `TODO.md` as required while also implying that topic files under `todo/` could replace it.
- 2026-03-16 - Updated `AGENTS.md`, `README.md`, and `memory/TodoAndLogWorkflow.md` to define two explicit queue modes: `TODO.md` mode and `todo/` mode.
- 2026-03-16 - Documented loop semantics for both cases: a specific topic-file run works only from that file, while a repo-wide run in `todo/` mode processes actionable topic files in priority order.

## [DONE 2026-03-16] Easier start with IFE
I received following feedback from colleague:
> Przydałby się jakiś boilerplate maker :D strasznie się namęczyłem z simlinkami i kontenerem 😛
Jakiś npx ief init-project "nazwa"

Think how we can improve starting with IFE. Maybe npx but maybe README is not clear enough? He went for workspace level setup which is a little bit more complex.

### Log
- 2026-03-16 - Diagnosis: the main problem is default-path clarity, not only missing tooling; the README exposed the advanced workspace setup too prominently.
- 2026-03-16 - Updated `README.md` to add a simple single-repo quick start and renamed the multi-repo container setup section to an explicitly advanced path.
- 2026-03-16 - Added `memory/StarterExperiencePlan.md` with a staged plan: improve docs first, then if still useful add two explicit bootstrap commands, `npx ief init-project` and `npx ief init-workspace`.

## [DONE 2026-03-15] Switch IEF to TODO-local logging
Decision: keep execution notes under the relevant TODO item, either inline in `TODO.md` or in a linked `todo/.../*.md` file.

### Log
- 2026-03-15 - Updated `AGENTS.md`, `README.md`, `skills/README.md`, and the Oracle session guidance so new work logs live with the task rather than in a separate required `LOG.md`.
- 2026-03-15 - Reworked `memory/TodoAndLogWorkflow.md` to make TODO-local logs the default model and kept `LOG.md` only as a historical archive.

## [DONE 2026-03-15] Suggest alternative names for IEF
See `memory/IefNamingIdeas.md`.

### Log
- 2026-03-15 - Added both explicit names and more product-style names closer to the naming style used in Peter Steinberger projects.

## [DONE 2026-03-15] Add MIT licensing information to IEF repo

## [DONE 2026-03-15] TODOs and LOGs
* Combine TODO with LOGS? - I think it makes sense to have logs under TODO items. Especially for Human this is easier to review than two separate files.
* I opt for supporting also alternative style of TODOs - each todo in separate md file under todo directory. IFE todos I would keep as they are now but at work I ended up with todo items as md files which contains quite some text and are also structured in subfolders like "doing now", "doing today", "near-term parked", "mid-term parked". What do you think? Maybe also allow for mix?

## [DONE 2026-03-15] ife/skill/README.md and ife/README.md improvements
  * "Relationship to other repo files" / "Memory layout" - I think we should have description of control-plane files/memory layout in once place. I think best in AGENTS.md and ife/skill/README.md and ife/README.md should just refer to it.
  * "Optional skills" as chapter in README.md is to much. I think it is enough that we've mentioned them in Vision chapter.

## [DONE 2026-03-15] Add to AGENTS.MD instruction that agent should report problems with invoking tools e.g. wanted to use rg in terminal but it was not available with information about the impact of this missing tool.

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

## [FOR HUMAN] Review workplace/always-on-agents-lab

## [FOR HUMAN] Test Opus 4.6 in this repo

## [DONE 2026-03-09] Check Oracle API cost vs ChatGPT Plus
research what additional cost API-mode Oracle introduces beyond a ChatGPT Plus subscription and document the practical cost trade-offs.

## [DONE 2026-03-09] Plan next Oracle integration steps
turn the existing Oracle research into a concrete implementation roadmap for IEF.

## [DONE 2026-03-09] Execute first Oracle integration steps
apply the roadmap in-repo, starting with the smallest useful integration artifacts.

## [DONE 2026-03-09] Check https://github.com/GoogleCloudPlatform/generative-ai/tree/main/gemini/agents/always-on-memory-agent
How we could use it in IEF? Can it or the idea be used without API keys but with agent doing Ingestion and Consolidation? How this is different from summarization process that most chats and agent platforms do?

## [DONE 2026-03-09] TODO and LOG location - part2
I'm still not convince that keeping them under memory is the best place. Readme is also memory. Maybe put it close to readme and reserve memory/ for other things like researches and memory based on concepts like "always-on-memory-agent".

## [DONE 2026-03-09] New project
Create a new project under workspace for finding best setup for having agents always on. E.g. I can run graphical session from my laptop "localy" where I have my agents running. But I want also be able to connect to it from other computer when I'm not at home. Best it would be if I can connect using web browser without installing anything. I tried RustDesk from webbroser but it was a little bit lagish.
How Peter Steinberg setup looks like in this respect?
Similar setup with linux?
BTW: Can I move GitHub Copilot sessions from one computer to another? What about codex and claude code.
