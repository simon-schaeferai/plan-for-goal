---
name: plan-for-goal
description: Macht aus einem fuzzy Ziel einen maschinell prüfbaren Goal-Plan (Markdown-Contract) für lange autonome Agent-Läufe — Ampel-Fit-Check (loop-geeignet vs. Human-in-the-Loop), Definition of Done, Acceptance Criteria mit Prüfmethode, Verification-Kommandos, Turn-/Zeit-Budget, Abbruch- und Eskalationsregeln. Nutze diesen Skill IMMER, wenn der User einen langen autonomen Lauf vorbereiten will oder ein Ziel formuliert, das ein Agent alleine abarbeiten soll — „plan for a goal", „Goal-Plan", „definier das Ziel für den Agenten", „soll alleine durchlaufen", „über Nacht arbeiten lassen", „Nachtschicht-Auftrag", „bereite den Lauf vor", „mach daraus ein Goal" — auch wenn das Wort „Plan" nicht wörtlich fällt.
---

# plan-for-goal · Fuzzy Ziel → prüfbarer Goal-Plan

Ein Goal-Plan ist der **Contract zwischen Mensch und autonom laufendem Agenten**. Der Agent arbeitet in einer Schleife (Turn → Selbst-Check gegen die Zielbedingung → weiter oder Stopp) und niemand schaut zu. Deshalb entscheidet die Qualität des Plans über den ganzen Lauf: Eine vage Zielbedingung („mach es besser") gibt dem Evaluator nichts zum Prüfen — der Lauf brennt Tokens und liefert Zufall. Eine binäre, maschinell prüfbare Definition of Done macht denselben Lauf zuverlässig.

Kernprinzip: **Ohne prüfbare Definition of Done gibt es keinen Lauf.** Lieber ehrlich sagen „das taugt nicht für einen Loop" als einen Schein-Plan bauen.

## Der Flow

### Schritt 0 · Goal finden (wenn der User nur ein Problem mitbringt)

Der User muss KEIN fertiges Goal liefern — ein Problem („ich verliere jede Woche Zeit mit X", „das nervt mich") oder ein vager Wunsch ist der Normalfall, kein Fehler. Die Definitionsarbeit ist der Job dieses Skills, nicht des Users. Wenn noch kein greifbares Goal da ist: Leite aus dem Problem 1–3 Kandidaten-Goals ab, jedes mit einem Ein-Satz-DoD-Entwurf und einer ehrlichen Ampel-Tendenz, und lass den User wählen, bevor du weitermachst. Formuliere die Kandidaten als das, was der Agent TUT — nicht als das, was der User sich wünscht.

### Schritt 1 · Fit-Check (Ampel) — IMMER zuerst

Lies [references/fit-check.md](references/fit-check.md) und stufe das Ziel ein, BEVOR irgendetwas geplant wird:

- **🟢 Grün** — mechanisch + maschinell verifizierbar (Migration, Bulk-Generierung, Triage, Datenaufbereitung) → voller Goal-Plan.
- **🟡 Gelb** — enthält autonome UND Geschmacks-Anteile → **Split**: Der Plan deckt nur den autonomen Teil ab; Auswahl, Urteil und alles Außenwirksame (posten, versenden, löschen) bleiben explizit beim Menschen und stehen als „Out of Scope / HITL" im Plan.
- **🔴 Rot** — lebt von Judgment, Taste oder Richtungsentscheidung → **ehrlich abraten** und die interaktive Alternative anbieten. Kein Plan.

Warum so streng: Ein Loop kann Arbeit wiederholen, aber nicht schmecken. Judgment, Taste und Direction bleiben beim Menschen — ein Plan, der das ignoriert, produziert autonome Fehlentscheidungen statt autonomer Arbeit.

### Schritt 2 · Kontext ernten statt fragen

Zieh alles selbst, was das Projekt schon weiß, bevor du eine einzige Frage stellst:

- CLAUDE.md / Projekt-Rules → Stack, eiserne Regeln, Constraints
- Vorhandene Verifikations-Infrastruktur → Test-Runner, Linter, Build-Kommandos, Browser-Checks (das werden die Verification-Kommandos)
- Bestehende Docs/Memories, die das Ziel berühren → References

Der User hat das Projekt schon einmal erklärt; er soll es nicht für jeden Goal-Plan wiederholen müssen.

### Schritt 3 · Mini-Interview (max. 5 Fragen)

Frag nur echte Forks, die der Kontext nicht hergibt. **Zwei Felder sind Pflicht-Bestätigung durch den User**, weil sie den Lauf beenden können:

1. **Definition of Done** — der User muss den einen binären Satz absegnen (das ist das Abbruchkriterium des Loops).
2. **Budget** — max. Turns bzw. Laufzeit, in intuitiven Einheiten („max. 4 Stunden", „max. 50 Turns"), keine Sub-Cent-Rechnungen.

Kein Rückkanal (Batch/Cron/Headless)? → Konservative Annahmen treffen und im Plan unter **Annahmen** offen ausweisen, statt zu blockieren oder still zu raten.

### Schritt 4 · Plan schreiben

Lies [references/goal-plan-template.md](references/goal-plan-template.md) und fülle ALLE Sektionen. Leere Sektionen sind verboten — wenn eine Sektion wirklich nicht zutrifft, steht dort warum (eine Zeile), nicht nichts.

### Schritt 5 · Qualitäts-Gate → speichern → übergeben

Prüfe den fertigen Plan gegen die Rubric (unten). Erst bei Bestehen speichern:

```
goals/YYYY-MM-DD-<slug>.md    (im Projekt-Root; Ordner anlegen falls nötig)
```

Dann übergeben: Zeig dem User den Plan-Pfad, die Definition of Done und das Startkommando aus der Sektion „Lauf-Setup". **Starte den Lauf niemals selbst** — der Plan ist die Vorbereitung, der Mensch zündet. Das gilt auch, wenn der User es eilig hat: „Plan liegt hier, so startest du ihn" ist das Ende dieses Skills.

## Qualitäts-Rubric (alle 6 müssen bestehen)

| # | Check | Warum |
|---|---|---|
| 1 | Definition of Done ist EIN Satz und binär entscheidbar (erfüllt / nicht erfüllt) | Der Loop-Evaluator braucht eine Ja/Nein-Bedingung |
| 2 | Jedes Acceptance Criterion nennt seine Prüfmethode (Kommando, Datei-Existenz, Zählwert) | „Sieht gut aus" ist keine Prüfung |
| 3 | Verification enthält ausführbare Kommandos, keine Absichten | Der Agent muss beweisen, nicht behaupten |
| 4 | Budget gesetzt + Abbruchregel definiert (was passiert bei Budget-Ende ohne DoD) | Loops sind Token-Burner; ohne Deckel brennt es durch |
| 5 | Eskalationsregel vorhanden (wann stoppen und fragen: destruktive Ops, alles Außenwirksame) | Autonomie endet, wo Schaden irreversibel wird |
| 6 | Scope hat eine explizite Out-Liste (inkl. HITL-Anteile bei Gelb) | Was nicht ausgeschlossen ist, macht ein Agent irgendwann |

Fällt ein Check durch → Plan nachbessern, nicht durchwinken. Die häufigste Schwäche ist eine DoD, die in Wahrheit ein Wunsch ist („Code ist sauber") — dann zurück zu Schritt 3 und den User nach dem messbaren Kern fragen.

## Verhalten

- **Challenge statt Gefälligkeit:** Wirkt das Ziel loop-ungeeignet, sag es begründet — ein verhinderter 5-Stunden-Fehllauf ist mehr wert als ein höflicher Plan.
- **Constraints erben:** Eiserne Regeln des Projekts (z. B. „nie automatisch posten", „nie Daten löschen", „Test-Versand nur an interne Adresse") wandern wörtlich in die Constraints-Sektion. Der Goal-Plan darf nie lockerer sein als das Projekt.
- **Wiederkehrende Aufgaben erkennen:** „Jede Nacht soll…" ist kein One-Shot-Goal, sondern eine Routine. Der Plan beschreibt dann den EINEN Lauf (idempotent, damit er jede Nacht neu starten kann) und das Lauf-Setup nennt den Scheduler (Cron/launchd) als Harness.
- **Nichts erfinden:** Verification-Kommandos nur nennen, wenn sie im Projekt existieren (nachschauen!). Fehlt Test-Infrastruktur, steht das als Risiko im Plan statt eines erfundenen `npm test`.

## Grenzen (ehrlich)

- Der Skill plant Läufe, er führt sie nicht aus und überwacht sie nicht.
- Ein perfekter Plan macht ein schlechtes Ziel nicht gut — Rot bleibt Rot.
- Budget-Schätzungen sind grob; der erste echte Lauf kalibriert sie.
