## [FOR HUMAN]
* Review and cleanup documents under /docs
* What do I need to fully delegate project execution (I'm only PO without looking at code)
  * depend on project for sure but let's focus on flowlite
  * what agents still need to have to be able continuously run the loops, generate new todo items, respond to events (prs, bugs, incidents on test env)
  * Agent activities:
    * writing reports for me

## [NEW] Feedback after review
* Add incentive for agent to add new todos to the leadger if it spots thing worth doing not yet present in the leadger.
* not sure if we explain in AGENTS.md and readme.md clear enough what is the difference between "Worker sessions and persistent specialist roles"
* Add info that if main agent can reuse existing context of role agent it shoud do it instead creating fresh one. Unless i decide that for current task it's onw forked context is better then role contex from previous work delegation.
* Add info to readme that for now we use native subagent mechnism of Agentic CLI but in future agents may be seperate processes
* It seems to me that "Agent work loop and iteration rhythm" chapter requires update after w introduced multi-agent setup.

## [NEW] Review
* Review whole README and AGENTS.md for things that may require improvements
