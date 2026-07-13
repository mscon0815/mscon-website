# MSCon Website — CLAUDE.md

## Projektkontext

Digitale Visitenkarte für MSCon (IT-Architektur & Digitale Transformation, DACH).
Stack: Vanilla HTML/CSS/JS + Three.js (CDN). Kein Build-Tool, kein Framework.
Hosting: Cloudflare Pages, deployt automatisch bei Push.

| Branch | Domain | Inhalt |
|---|---|---|
| `main` | mscon.one | Construction-Page + Legal-Seiten |
| `staging` | staging.mscon.one | Voller One-Pager (Zielzustand) + Legal-Seiten |

Dateien: `index.html`, `impressum.html`, `datenschutz.html`, `404.html`, `_redirects`, `assets/`.

## Prinzipien

### Workflow

1. **Repo-Sync vor Arbeit:** Immer zuerst `git fetch` + Abgleich mit origin
   (main UND staging). Zieldateien können lokal fehlen oder veraltet sein.
2. **Plan/Bericht-Konvention:** Jedes Vorhaben bekommt ein File in
   `docs/plans/YYYY-MM-DD-<slug>.md` (Template: `docs/plans/README.md`).
   Plan vor der Umsetzung schreiben, Tests + Ergebnisse nachtragen,
   Status-Header pflegen.
3. **Selbst-Test vor Push:** Verifikations-greps, i18n↔DOM-Konsistenz
   (de/en-Key-Mengen identisch und deckungsgleich mit `t-*`-IDs),
   Playwright-Smoke mit `channel: 'chrome'` gegen lokalen Server
   (`python3 -m http.server`). Erst pushen, wenn alles grün ist.
4. **Live-Check nach Deploy:** Nach jedem Push curl-Checkliste gegen
   mscon.one und staging.mscon.one (Deploy-Polling einplanen).

### Projekt

5. **Bilinguale Pflege DE/EN:** Jede Inhaltsänderung ändert das sichtbare
   DE-HTML UND das i18n-Objekt (`de` + `en`) in derselben Datei synchron.
   Es gibt keine separaten EN-Dateien. HTML-Elemente und i18n-Keys immer
   zusammen anlegen/entfernen.
6. **Rechtsseiten-Regeln:** Rechtsgrundlage ist `§ 5 DDG` (nie TMG);
   keine Steuernummer veröffentlichen; `§18 Abs. 2 MStV` bleibt;
   Haftungssätze ohne Paragraphen-Nennung. Adresse: Weberstr. 6,
   52441 Linnich. E-Mail: schultes@mscon.one.
7. **Commit-Hygiene:** Thematisch getrennte Commits (Conventional Commits:
   `fix`/`feat`/`docs`/`chore`), WIP via Stash isolieren, keine vermischten
   Commits. Attribution ist global deaktiviert.

## Design-System (nicht ändern)

```css
--bg:#070707  --red:#9B2335  --gold:#E8A030  --olive:#7A8C3A
--text:#f0ece4  --dim:#3a3530  --dim2:#5a5248  --border:rgba(255,255,255,.06)
```
Fonts: Fraunces (Display) · DM Sans (Body) · JetBrains Mono (Code/Scramble)

## Kritische Funktionen One-Pager (staging — nicht anfassen ohne Test)

- Cursor: `left/top` direkt + margin-offset `-5px/-17px`
- FOG: `fogMove(x,y)` → radial-gradient rebuild · UNLOCK: `body.open` nach 700ms
- CARDS: `tog(idx)` → scramble + unlock counter · CONTACT: bei `unlocked===6` → `#cwrap.on`
- Nach jeder Änderung Cursor + Unlock-Button im Browser testen (war mehrfach gebrochen)

## Offene Arbeit

Siehe `docs/plans/` (Status-Header) — aktuell:
- `2026-07-13-onepager-backlog.md` — SEO/A11y/DSGVO-Restpunkte (in Arbeit)
