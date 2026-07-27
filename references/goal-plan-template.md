# Goal-Plan-Template

Alle Sektionen ausfüllen. Reihenfolge einhalten — sie ist Lese-Reihenfolge für den ausführenden Agenten: erst verstehen (Briefing→Constraints), dann wissen wann Schluss ist (DoD→Verification), dann die Leitplanken (Budget→Eskalation).

## Das Template

```markdown
# Goal: <Titel in einem Satz>

> Status: bereit / läuft / abgeschlossen · Erstellt: YYYY-MM-DD · Ampel: 🟢/🟡

## Briefing
2–4 Sätze: Was gebaut/erreicht werden soll und WARUM (das Warum hilft dem
Agenten bei Mikro-Entscheidungen, die der Plan nicht vorhersieht).

## Lauf-Setup
- Harness: <Claude-Code-Loop /goal · Workflow · Cron/launchd · Subagent>
- Startkommando: <exaktes Kommando, mit dem der Mensch den Lauf zündet>
- Arbeitsverzeichnis: <Pfad>

## Stack
Werkzeuge, Sprachen, Repos, Keys/Umgebung — nur was der Lauf wirklich braucht.

## Scope
**In:** <konkrete Arbeitspakete>
**Out (explizit):** <was der Agent NICHT anfasst — inkl. aller HITL-Anteile
bei Gelb: Auswahl, Veröffentlichung, Versand, Löschungen>

## Constraints
Harte Grenzen, wörtlich aus den Projekt-Regeln geerbt + goal-spezifische.
Beispiele: „nie automatisch posten", „keine Migration ohne Backup",
„nur additive Schema-Änderungen".

## Annahmen
Nur bei Headless-Erstellung ohne Rückfrage-Möglichkeit: Welche Annahmen
wurden getroffen, die der User beim Review kippen kann. Sonst: „keine".

## Definition of Done
EIN binärer Satz. Der Loop endet genau dann, wenn dieser Satz wahr ist.

## Acceptance Criteria
- [ ] <Kriterium> — Prüfung: <Kommando / Datei-Existenz / Zählwert>
- [ ] <Kriterium> — Prüfung: <…>
(3–8 Stück. Jedes einzeln maschinell prüfbar. Zusammen ⇒ DoD.)

## Verification
Exakte Kommandos/Checks, die der Agent VOR der Fertig-Meldung selbst
ausführt, mit erwartetem Ergebnis:
1. `<kommando>` → erwartet: <exit 0 / Ausgabe X>
2. …

## Budget & Abbruchregel
- Budget: max. <N Turns / X Stunden Laufzeit>
- Bei Budget-Ende ohne DoD: sauber stoppen + Zwischenstand-Report schreiben
  (was fertig, was offen, woran es hing) — NIE still weiterbrennen.

## Eskalation (stoppen und fragen statt handeln)
Der Agent unterbricht den Lauf und meldet sich, wenn:
- eine destruktive Operation nötig scheint (löschen, überschreiben, force-push)
- etwas Außenwirksames ansteht (posten, versenden, deployen, fremde Systeme)
- <goal-spezifische Trigger>

## References
Dateien, Docs, Beispiele, die der Agent lesen soll (mit Pfaden).

## Risks / Open Questions
Ehrlich: was schiefgehen kann, was ungeklärt ist. Fehlende
Test-Infrastruktur gehört hierhin — nicht als erfundenes Kommando in
die Verification.
```

## Feld-Anleitung (die häufigsten Fehler)

- **DoD ist das Abbruchkriterium, kein Marketing.** Test: Könnte ein kleines Modell mit dem Transkript des Laufs entscheiden, ob der Satz wahr ist? „Alle 12 Scripts laufen unter ESM und `npm test` ist grün" → ja. „Die Codebase ist modernisiert" → nein.
- **Acceptance Criteria sind die DoD in Einzelteilen.** Wenn alle Häkchen gesetzt sind, MUSS die DoD wahr sein — sonst fehlt ein Kriterium.
- **Verification ist Beweis, nicht Absicht.** „Agent testet gründlich" ist Absicht. „`npm test` → exit 0" ist Beweis. Kommandos vorher im Projekt verifizieren (package.json, Makefile lesen) — ein erfundenes Kommando lässt den Loop endlos gegen eine Wand laufen.
- **Out-Liste schlägt In-Liste.** Agenten erweitern Scope von selbst; nur was explizit ausgeschlossen ist, bleibt sicher liegen.
- **Budget in intuitiven Einheiten.** „Max. 4 Stunden" oder „max. 50 Turns", grobe Kosten nur wenn relevant und dann in ganzen Beträgen pro Lauf — keine Sub-Cent-Dezimalwerte.

## Ausgefülltes Beispiel (gekürzt)

```markdown
# Goal: report-cli von CommonJS auf ES Modules migrieren

> Status: bereit · Erstellt: 2026-07-26 · Ampel: 🟢

## Briefing
Das interne CLI-Tool in tools/report-cli nutzt noch require/module.exports.
Migration auf ESM, damit es moderne Dependencies (chalk 5+) nutzen kann.
Nachtlauf; morgens wird nur noch das Diff reviewt.

## Lauf-Setup
- Harness: Claude-Code-Loop (/goal)
- Startkommando: claude "/goal @goals/2026-07-26-report-cli-esm.md"
- Arbeitsverzeichnis: tools/report-cli

## Stack
Node 20, npm. Tests: `npm test` (vitest). Lint: `npm run lint` (eslint).

## Scope
**In:** alle .js unter tools/report-cli/src + bin; package.json ("type":
"module"); Imports/Exports; __dirname-Ersatz.
**Out (explizit):** keine neuen Features, kein Dependency-Upgrade, keine
Änderungen außerhalb von tools/report-cli, kein Commit/Push.

## Constraints
Nur additive/umschreibende Änderungen — keine Datei löschen. Öffentliche
CLI-Flags bleiben identisch.

## Annahmen
keine (interaktiv erstellt)

## Definition of Done
Alle Dateien unter tools/report-cli nutzen ESM-Syntax und `npm test` sowie
`npm run lint` enden mit Exit-Code 0.

## Acceptance Criteria
- [ ] Kein `require(` mehr in src/ + bin/ — Prüfung: `grep -r "require(" src bin` → leer
- [ ] package.json enthält `"type": "module"` — Prüfung: grep
- [ ] `npm test` grün — Prüfung: exit 0
- [ ] `npm run lint` grün — Prüfung: exit 0
- [ ] CLI startet: `node bin/report.js --help` → exit 0

## Verification
1. `grep -rn "require(" src bin` → keine Treffer
2. `npm test` → exit 0
3. `npm run lint` → exit 0
4. `node bin/report.js --help` → usage-Text, exit 0

## Budget & Abbruchregel
- Budget: max. 4 Stunden Laufzeit
- Bei Budget-Ende ohne DoD: Stopp + Report nach goals/reports/, offene
  Dateien auflisten.

## Eskalation (stoppen und fragen statt handeln)
- Test schlägt fehl aus Gründen, die VOR der Migration schon bestanden
- eine Abhängigkeit müsste geupgradet werden (Out of Scope)
- Löschen einer Datei scheint nötig

## References
- tools/report-cli/package.json · Node-Docs ESM-Migration

## Risks / Open Questions
- vitest-Konfiguration könnte CJS-gebunden sein → dann zuerst vitest.config
  prüfen, bevor Massen-Umbau startet.
```
