## [FOR HUMAN]
* Review and cleanup documents under /docs
* What do I need to fully delegate project execution (I'm only PO without looking at code)
  * depend on project for sure but let's focus on flowlite
  * what agents still need to have to be able continuously run the loops, generate new todo items, respond to events (prs, bugs, incidents on test env)
  * Agent activities:
    * writing reports for me

## [DONE 2026-08-29.1] Feedback after review
* Add incentive for agent to add new todos to the leadger if it spots thing worth doing not yet present in the leadger.
  > Agent: Dodano zasadę `Backlog discovery`: agent zapisuje wartościowe i wykonalne odkrycie jako `NEW` z uzasadnieniem, ale nie rozszerza po cichu bieżącego kontraktu ani nie zmienia automatycznie priorytetu. Worker zapisuje takie odkrycia we własnym ledgerze, a `main` rozstrzyga je przy handoffie. README komunikuje tę możliwość również użytkownikowi.
* not sure if we explain in AGENTS.md and readme.md clear enough what is the difference between "Worker sessions and persistent specialist roles"
  > Agent: Różnica nie była wystarczająco wyraźna. Oba dokumenty definiują teraz trzy osobne pojęcia: trwała rola to specjalizacja i pamięć w repo, worker session to tymczasowe wykonanie modelu/runtime'u, a workstream to ograniczony wynik. Jedna rola może mieć wiele kolejnych sesji i workstreamów; jednorazowy workstream nie wymaga tworzenia roli.
* Add info that if main agent can reuse existing context of role agent it shoud do it instead creating fresh one. Unless i decide that for current task it's onw forked context is better then role contex from previous work delegation.
  > Agent: Dodano preferencję ponownego użycia istniejącej sesji roli, jeśli jej uchwyt nadal jest dostępny, kontekst jest trafny i jakościowy, a sesja nie wykonuje konfliktującej pracy. Świeży kontrolowany fork jest właściwy dla kontekstu niedostępnego, starego/zaszumionego albo zadania zależnego głównie od nowych decyzji `main`. Oficjalna dokumentacja Codex opisuje sterowanie widocznymi wątkami w bieżącym kliencie, lecz nie gwarantuje przetrwania natywnego uchwytu po restarcie; jawne wznawianie jest udokumentowane dla `codex exec resume`.
* Add info to readme that for now we use native subagent mechnism of Agentic CLI but in future agents may be seperate processes
  > Agent: README opisuje natywnych podagentów jako bieżącą implementację referencyjną, nie wymóg IEF. Osobne procesy CLI, CI, usługi, worktree lub inne backendy mogą przejąć role bez zmiany repozytoryjnego control plane.
* It seems to me that "Agent work loop and iteration rhythm" chapter requires update after w introduced multi-agent setup.
  > Agent: Diagram obejmuje teraz decyzję `main` o delegacji, zapis kontraktu, osobny loop workera, commit i handoff, niezależny odbiór/integrację przez `main` oraz dopiero potem zamknięcie głównego elementu i przejście do kolejnego. Ścieżka blokady workera wraca przez `main` do człowieka.

> Validation: `git diff --check`; ręczna kontrola spójności terminów i wszystkich pięciu uwag między README, sekcjami Roles/Multi-agent coordination/Agent work loop w AGENTS.md oraz oficjalną dokumentacją OpenAI Subagents i Non-interactive mode.

## [DONE 2026-08-29.1] Review
* Review whole README and AGENTS.md for things that may require improvements
  > Agent: Przejrzano oba dokumenty od początku do końca po zmianach z poprzedniego elementu. README ma teraz prostszy opis pamięci projektu, poprawiony quick start i przykładowe prompty, ostrożne wymagania wobec kompatybilnych CLI/IDE zamiast obietnicy, że każde narzędzie zachowa się identycznie, poprawne linki/formatowanie oraz bez datowanego wpisu o prywatnie używanym modelu.
  > Changes: AGENTS.md wymaga teraz outcome-based kontraktu z ownershipem, zakazanym zakresem, uprawnieniami i evidence, zacommitowanego przed startem workera. Diagram obsługuje odrzucony handoff, poprawki workera, odbiór, release i routing blokady przez `main`. Dodano bezpieczeństwo równoległej pracy, rozdzielenie monitoringu od sesji specjalisty, precyzyjny commit tylko plików danego elementu, routing eskalacji i poprawki językowe/formatujące.
  > Validation: `git diff --check`; wszystkie lokalne cele linków istnieją; oba odnośniki do oficjalnej dokumentacji OpenAI zwracają HTTP 200; ręczne przejście całych README i AGENTS.md pod kątem sprzeczności między quick startem, control plane, delegacją, diagramem, hard gate i logowaniem. Nie dodano nowego TODO: jedyne pozostałe zagadnienie (`[FOR HUMAN]`) już istnieje w ledgerze i nie powinno być duplikowane.
