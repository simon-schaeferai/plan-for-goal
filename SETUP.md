# Setup

Der Skill ist reines Markdown. Es gibt nichts zu installieren außer Claude Code selbst.

## Voraussetzungen

- [Claude Code](https://claude.com/claude-code) (CLI, Desktop-App oder VS-Code-Extension)
- Sonst nichts: keine API-Keys, keine Dependencies, kein Build

## Installation

```bash
# Global (in allen Projekten verfügbar)
git clone https://github.com/simon-schaeferai/plan-for-goal.git ~/.claude/skills/plan-for-goal

# Oder projekt-lokal (nur ein Repo)
git clone https://github.com/simon-schaeferai/plan-for-goal.git .claude/skills/plan-for-goal
```

Prüfen:

```bash
ls ~/.claude/skills/plan-for-goal/SKILL.md   # Datei da → Skill installiert
```

In einer neuen Claude-Code-Session taucht der Skill dann als `/plan-for-goal` auf
(Skills werden beim Session-Start eingelesen; laufende Sessions einmal neu starten).

## Erster Lauf

```
/plan-for-goal Schreib mir zehn LinkedIn-Posts aus meinen Notizen
```

Claude stellt die Fragen aus dem Flow (Fit-Check zuerst), schreibt den Plan nach
`goals/YYYY-MM-DD-<slug>.md` und zeigt dir Definition of Done plus Startkommando.
Den Lauf startest du selbst — der Skill zündet nie.

## Troubleshooting

| Symptom | Ursache | Fix |
|---|---|---|
| `/plan-for-goal` erscheint nicht | Session lief schon beim Clone | Session neu starten |
| Skill schreibt in den falschen Ordner | `goals/` gehört in den Projekt-Root | im Projekt-Root starten, nicht in einem Unterordner |
| Plan wird trotz vager DoD gespeichert | Rubric-Schritt übersprungen | in SKILL.md Schritt 5 zeigen: alle 6 Checks müssen bestehen |
