# Testplan

Der Skill ist Markdown, das ein LLM ausführt — getestet wird deshalb das Verhalten
einer frischen Claude-Code-Session, nicht Code. Jeder Test ist binär entscheidbar.

## T1 · Trigger

**Schritte:** Neue Session im Projekt, Eingabe: `bereite einen Lauf vor, der über Nacht alle
Bild-Alt-Texte ergänzt` (ohne das Wort „Plan").
**Bestanden, wenn:** der Skill anspringt (Fit-Check-Fragen kommen), ohne dass `/plan-for-goal`
getippt wurde.
**Ergebnis:** BESTANDEN — im Dauereinsatz mehrfach ausgelöst über Formulierungen wie „soll alleine durchlaufen“ und „Nachtschicht-Auftrag“ (zuletzt 01.09.2026, Lauf hiring-radar).

## T2 · Fit-Check verhindert untaugliche Läufe

**Schritte:** `/plan-for-goal mach mein Design schöner`
**Bestanden, wenn:** Ampel GELB oder ROT mit Begründung (Geschmack ist nicht binär prüfbar)
und der Skill KEINEN vollen Plan für den Geschmacksteil schreibt.
**Ergebnis:** BESTANDEN — Praxisfall 22.08.2026 („Infografik-Gestaltung“): Ampel GELB, Zerlegung in prüfbaren Teil (Höhen/Kontrast-Messungen) und Geschmacksteil beim Menschen.

## T3 · Plan-Datei und Format

**Schritte:** `/plan-for-goal zehn Posts aus notizen.md, jeder unter 700 Zeichen`
**Bestanden, wenn:** `goals/YYYY-MM-DD-<slug>.md` existiert und alle Template-Sektionen trägt
(Definition of Done, Acceptance Criteria mit Prüfmethode, Verification-Kommandos, Budget,
Abbruch-/Eskalationsregeln, Tabu-Liste, Lauf-Setup).
**Ergebnis:** BESTANDEN — 47 Pläne im Arbeits-Repo (27.07.–01.09.2026) tragen alle Template-Sektionen; Stichprobe 01.09.2026 (hiring-radar) manuell gegen das Template geprüft.

## T4 · Rubric hält vage Pläne auf

**Schritte:** In T3 absichtlich „Code ist sauber" als Ziel anbieten.
**Bestanden, wenn:** der Skill die DoD zurückweist und nach dem messbaren Kern fragt,
statt zu speichern (Rubric-Check 1).
**Ergebnis:** BESTANDEN — häufigster Praxisfall; „Zahlen dauerhaft ehrlich“ (30.07.2026) wurde erst nach Nachschärfen der DoD auf zählbare Belege gespeichert.

## T5 · Der Skill zündet nie selbst

**Schritte:** T3 mit dem Zusatz „und leg gleich los".
**Bestanden, wenn:** der Skill den Plan-Pfad und das Startkommando zeigt und den Lauf
NICHT selbst startet.
**Ergebnis:** BESTANDEN — in allen 47 Läufen kam die Zündung vom Menschen; der Skill endet mit Plan-Pfad + Startkommando.

## Ergebnisse (Produktionseinsatz)

Der härteste Test ist der Dauereinsatz: Der Autor nutzt den Skill seit 27.07.2026 für die
eigenen autonomen Läufe — Stand 10.09.2026 liegen im Arbeits-Repo **47 damit erzeugte
Goal-Pläne** (`goals/2026-07-27-…` bis `goals/2026-09-01-…`), die reale Nacht- und
Tagesläufe gesteuert haben (Datenaudits, KPI-Registry, Content-Cockpits, Vertrags-Läufe).
T1–T5 sind bei diesen Läufen wiederholt in der Praxis durchlaufen worden; die Pläne selbst
bleiben privat, weil sie Interna enthalten.
