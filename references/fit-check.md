# Fit-Check: Taugt dieses Ziel für einen autonomen Loop?

Die Ampel entscheidet, ob überhaupt geplant wird. Die Kernfrage ist immer dieselbe: **Kann eine Maschine am Ende prüfen, ob das Ziel erreicht ist — ohne menschlichen Geschmack?**

Zweite Frage direkt danach: **Ist irgendein Schritt außenwirksam oder irreversibel?** (posten, versenden, deployen, löschen, Kunden kontaktieren) → Diese Schritte sind NIE Teil des autonomen Scope, egal wie grün der Rest ist.

## 🟢 Grün — voller Goal-Plan

Mechanische Arbeit mit maschinell prüfbarem Ergebnis. Der Loop spielt seine Stärke aus: Wiederholung ohne Ermüdung.

| Muster | Beispiele | Typische DoD |
|---|---|---|
| Migration/Refactoring | CJS→ESM, API-Umzug, Framework-Update | Tests + Lint grün, altes Muster nicht mehr grepbar |
| Bulk-Generierung nach Spec | 50 Produktseiten aus CSV, Newsletter-Draft-Varianten | N Dateien existieren + Schema-Check besteht |
| Daten-Triage/-Aufbereitung | Inbox sortieren, Duplikate mergen, CSV normalisieren | 0 unbearbeitete Items, Regel-Checks grün |
| Fehlerjagd bis Schwelle | „Fixe Lint-Errors bis 0", Flaky-Tests stabilisieren | Zähler = 0, CI grün |

## 🟡 Gelb — Split: autonomer Teil geplant, Rest bleibt HITL

Das Ziel enthält beides: mechanische Produktion UND Geschmacks-/Außenwirkung. Nicht abraten — **zerlegen**:

1. Autonomer Teil → in den Goal-Plan (z. B. „generiere 10 Post-Entwürfe nach Voice-Guide in drafts/")
2. Menschlicher Teil → explizit in **Scope: Out** mit Begründung (z. B. „Auswahl der besten 3 + Veröffentlichung: HITL — Geschmack und Außenwirkung")

Typische Gelb-Fälle: Content-Produktion (Generieren grün, Kuratieren/Posten menschlich) · Kundenkommunikation (Entwürfe grün, Versand menschlich) · Design-Varianten (Erzeugen grün, Auswahl menschlich).

Merkregel: **Der Agent produziert Kandidaten, der Mensch entscheidet und veröffentlicht.**

## 🔴 Rot — ehrlich abraten, kein Plan

Das Ziel IST die Geschmacks- oder Richtungsentscheidung. Ein Loop kann hier nur Zufall oder Blender produzieren, und die DoD wäre zwangsläufig fake („bis es gut ist").

Typische Rot-Fälle:
- „Finde heraus, welche Strategie/Positionierung wir fahren sollten"
- „Mach das Design schöner" (ohne messbare Spec)
- „Entscheide, welche Leads wir anrufen" (Urteil mit Konsequenz)
- Alles, dessen Erfolg erst Wochen später an Außenmetriken sichtbar wird

Antwort bei Rot: kurz begründen (fehlende prüfbare DoD bzw. Judgment/Taste/Direction gehören dem Menschen) + die interaktive Alternative anbieten („lass uns das gemeinsam in einer Session entscheiden — Vorbereitung dazu kann ich autonom machen: Daten sammeln wäre grün").

## Sonderfall: Wiederkehrende Aufgabe (Routine, kein One-Shot)

„Jede Nacht / jeden Morgen soll ein Agent …" ist kein einmaliges Goal, sondern eine Routine. Dann gilt:

- Der Plan beschreibt **einen einzelnen Lauf** und muss **idempotent** sein (zweimal starten = kein Schaden, nichts doppelt).
- DoD gilt **pro Lauf** („alle heute eingegangenen X sind verarbeitet"), nicht „für immer".
- Lauf-Setup nennt den Scheduler (Cron/launchd/Server-Agent) als Harness.
- Eskalation braucht einen asynchronen Kanal (Report-Datei, Slack, Log) — nachts fragt niemand zurück.

## Grenzentscheidungen (wenn du schwankst)

- Prüfbarkeit schlägt Bauchgefühl: Lässt sich JETZT ein konkretes Prüfkommando hinschreiben? Dann mindestens Gelb. Wenn nicht: Rot, egal wie „automatisierbar" es klingt.
- Bei Gelb-oder-Rot: Gibt es einen abtrennbaren Produktions-Anteil mit eigenem Wert? Ja → Gelb (Split). Nein → Rot.
- Im Zweifel die strengere Farbe wählen und das dem User transparent begründen — er kann hochstufen, wenn er das Risiko bewusst trägt.
