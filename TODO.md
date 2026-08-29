## [FOR HUMAN]
* Review and cleanup documents under /docs
* What do I need to fully delegate project execution (I'm only PO without looking at code)
  * depend on project for sure but let's focus on flowlite
  * what agents still need to have to be able continuously run the loops, generate new todo items, respond to events (prs, bugs, incidents on test env)
  * Agent activities:
    * writing reports for me

## [DONE 2026-08-29.2] Feedback 2
* I made some simplifications in README and Agents.md. What do you think about them?
  > Agent: Kierunek jest dobry. Jednozdaniowy prompt `Execute the IEF loop on <ledger>` wystarcza, ponieważ właściwy kontrakt jest w repo; usunięcie osobnej sekcji concurrency i zdania o „bounded sessions” także zmniejsza ciężar bez utraty głównego loopa. Dwie korekty były jednak potrzebne: `worker leader` było literówką i nie wyznaczało nawet minimalnych granic delegacji, a całkowite usunięcie ochrony współdzielonego checkoutu pozostawiało realne ryzyko mieszania commitów. Zostawiłem więc po jednym krótkim zdaniu na każdy z tych tematów. W README poprawiłem gramatykę quick startu i zachowałem zamysł szerokiej kompatybilności, formułując go jako wymaganie możliwości wczytania instrukcji i użycia potrzebnych narzędzi zamiast bezwarunkowej gwarancji. Datowany reference setup pozostał jawnie opisany jako reference setup.
* Add incentive for agents to report to human/main agent ideas for improvements in IEF if they spot something.
  > Agent: Dodano `Framework improvement feedback`. Konkretna obserwacja ulepszająca IEF jest teraz traktowana jako wartościowy wynik pracy i ma trafić do `main`/człowieka wraz z wpływem i propozycją. Agent posiadający ledger IEF dodaje `NEW`; worker innego projektu zapisuje pomysł w handoffie zamiast samowolnie zmieniać framework. Oddzielenie od backlogu projektu pozwala ocenić pomysł raz i wykorzystać go przekrojowo.
* Is it possible to simplify visualization of workflow by spliting it into two diagrams?
  > Agent: Tak. Jeden diagram pokazuje wyłącznie koordynację `main`: wybór, delegację lub wykonanie bezpośrednie, odbiór, ewentualny rework, integrację/release i hard gate. Drugi pokazuje lokalny loop workera: wybór własnego elementu, wykonanie, checkpoint blokady albo log/status/commit, kontynuację i handoff. Dzięki temu nie mieszamy dwóch poziomów stanu na jednej grafice.

> Validation: `git diff --check`; porównanie zmian MG w commicie `b17df09` z poprzednim kontraktem; ręczna kontrola, że uproszczony prompt nadal odwołuje się do zacommitowanego ledgera, oba diagramy pokrywają ścieżki wykonania/blokady/reworku, a nowe feedback rules nie przyznają workerowi prawa do edycji obcego frameworka.
