## [DONE 2026-08-30.1] Retencja zakończonych elementów ledgerów

Jak zarzadzac wielkością ledgera? Ja bym usuwał całkowicie stare DONE todo items po jakimś czasie (historia jest w git). Po jakim?
Przy usuwaniu agent sprawdza czy czegoś nie warto dodać do long term doc.
Każda rola robi to dla swojego ledgera.
Co myślisz o tym i jak to zaprojektować w IEF?

> Agent: Projekt ma utrzymać ledger jako bieżący control plane, a Git jako pełną historię. Domyślna retencja będzie łączyć wiek i minimalne okno kontekstu, z obowiązkową destylacją trwałej wiedzy przed fizycznym usunięciem wpisu.
> Decision: Wpis `DONE` można usunąć w całości, gdy ma co najmniej 30 dni i nie należy do 10 najnowszych zakończonych wpisów danego ledgera. Nigdy nie usuwa się pracy otwartej, zablokowanej, nadal referencjonowanej, z nierozliczonym feedbackiem ani niedostępnej w kanonicznej historii Git.
> Changes: Dodano normatywną politykę retencji i destylacji do `AGENTS.md` oraz krótkie objaśnienie do README. Przed usunięciem właściciel przenosi trwałe instrukcje, decyzje, learnings i follow-upy do README, `docs/`, rosteru albo nowych elementów kolejki. Każda rola sprząta własny ledger po commicie pracy, przed handoffem; `main` sprząta swój. Cleanup jest osobnym commitem, bez tombstone'ów i bez mieszania ze zmianą produktu.
> Validation: Sprawdzono kontrakt względem ledgerów głównego i ról, hard gate'u commitów, handoffów, otwartych komentarzy oraz historii Git. Reguła zachowuje co najmniej 30 dni i 10 ostatnich wyników, a jednocześnie pozwala rzeczywiście zmniejszać plik zamiast tworzyć drugi rosnący archiwum-ledger.
> Stop: Element frameworkowy jest ukończony; pozostałe otwarte punkty `[FOR HUMAN]` nie są wykonywalnymi elementami ze statusem IEF i pozostają do decyzji człowieka.

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
