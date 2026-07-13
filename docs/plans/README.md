# docs/plans/ — Plan- & Berichtsablage

Ein Markdown-File pro Vorhaben. Plan, Umsetzung, Tests und Ergebnisse leben
im selben Dokument, damit nachvollziehbar bleibt, was geplant war und was
tatsächlich passiert ist.

## Konvention

- **Dateiname:** `YYYY-MM-DD-<slug>.md` (Datum = Start des Vorhabens)
- **Status-Header** in der ersten Zeile nach dem Titel:
  - `Status: in Arbeit`
  - `Status: umgesetzt ✅`
  - `Status: verworfen ❌` (mit Begründung unter „Offene Punkte")
- **Sprache:** Deutsch
- Ein Vorhaben = ein File. Folgearbeiten bekommen ein neues File mit Verweis
  auf das alte.

## Pflicht-Sektionen (Template)

```markdown
# <Titel des Vorhabens>

Status: in Arbeit
Branch/Commits: <branch> · <sha> …

## Kontext
Warum wird das gemacht? Auslöser, Problem, Ziel.

## Plan
Die geplanten Schritte (vor der Umsetzung geschrieben).

## Umsetzung
Was tatsächlich gemacht wurde, inkl. Abweichungen vom Plan.

## Tests
Wie wurde geprüft (Befehle, Scripts, Browser-Tests) — mit Ergebnis je Test.

## Ergebnisse
Endergebnis, Live-Verifikation, betroffene Dateien/Commits.

## Offene Punkte
Was bewusst nicht gemacht wurde, Beobachtungen, Folgearbeiten.
```
