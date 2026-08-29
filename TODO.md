## [FOR HUMAN]
* What do I need to fully delegate project execution (I'm only PO without looking at code)
  * Additional communication channels and loop triggers in addition to adding todo items to the leadger + issuing a prompt to cli
  * Incorporate agent that is responsible for production monitoring + SRE
  * what agents still need to have to be able continuously run the loops, generate new todo items, respond to events (prs, bugs, incidents on test env)
  * Agent activities:
    * writing reports for me?
* Review and cleanup documents under /docs


## [DONE 2026-08-29.3] Feedback 3
* Is it clear for you from AGENTS.md that main should do other items when task is delegated (should not wait for worker to finish unless no other items left in main leadger)? If not try to show that without complicating diagrams to much.
  > Agent: Nie było to dość jasne: bezpośrednia strzałka od uruchomienia workera do odbioru handoffu sugerowała synchroniczne czekanie. Diagram `main` ma teraz mały wybór „Other actionable main work?”: po delegacji lub odesłaniu reworku wraca do głównej kolejki, a przechodzi do oczekiwania tylko bez innej pracy. Końcowy wybór rozróżnia gotowy handoff, nadal działających workerów, własne zadania i całkowity brak pracy. Pod diagramem dodano jedną normatywną regułę: delegacja/rework są asynchroniczne, a `main` czeka na handoff lub zdarzenie zewnętrzne dopiero po wyczerpaniu innych wykonalnych elementów głównego ledgera.

> Validation: `git diff --check`; ręczne przejście diagramu dla trzech scenariuszy: worker działa równolegle z kolejnymi elementami `main`, handoff jest już gotowy do odbioru oraz worker jest jedynym pozostałym oczekiwaniem.
