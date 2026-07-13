# Rechts-Update Impressum/Datenschutz: DDG, neue Adresse, Steuernummer

Status: umgesetzt ✅
Branch/Commits: main `db41669` · staging `2329362` (cherry-pick)

## Kontext

Auslöser: Brief `IMPRESSUM-UPDATE_1.md` (Rechts-Update mscon.one). Das Impressum
nannte das im Mai 2024 durch das DDG abgelöste TMG, die alte Adresse
(Aachener Str. 19 b, 52349 Düren) und die nicht-impressumspflichtige
Steuernummer (Risiko Identitätsmissbrauch). Gleiches galt für die
staging-Subdomain (Branch `staging`, byte-identisches Impressum).

Vorgefundener Zustand:
- Lokales Repo war 5 Commits hinter origin/main; `impressum.html` /
  `datenschutz.html` existierten nur remote.
- Beide Seiten sind bilingual: sichtbares DE-HTML + JS-i18n-Objekt (`de`/`en`)
  in derselben Datei — die EN-Version (TODO #6) wird über dieselben Edits
  mitgepflegt, es gibt keine separaten EN-Dateien.
- `404.html` und `index.html`: keine betroffenen Angaben.

Entscheidungen:
- Keine USt-IdNr. vorhanden → Steuer-Abschnitt ersatzlos gestrichen.
- Kein Telefon → TODO #5 entfällt.
- TODO #4 (Cloudflare E-Mail-Obfuskierung deaktivieren) gestrichen — keine Aktion.
- Lokale, nicht zugehörige WIP-Änderungen an `index.html` (SEO-Meta,
  Self-hosted Fonts) via Stash über den Pull gerettet; bleiben uncommitted.

## Plan

1. Git-Sync: Stash → `git pull --ff-only origin main` → Stash pop
2. TODO #1: `§5 TMG` → `§ 5 DDG` (DE-HTML + i18n de/en); §7-TMG-Haftungssatz
   ohne Paragraphen-Nennung umformulieren (3 Stellen); `§18 Abs. 2 MStV` bleibt
3. TODO #2: Abschnitt „Steuerliche Angaben" (Steuernummer 207/…) entfernen,
   inkl. i18n-Keys `h3`/`tax-label` in de UND en
4. TODO #3: Adresse → Weberstr. 6, 52441 Linnich (impressum 2×, datenschutz 1×)
5. TODO #6: EN-Version — durch i18n-Edits miterledigt, per grep verifiziert
6. Nach jedem TODO: Verifikations-grep aus dem Brief
7. Selbst-Test lokal (statischer i18n↔DOM-Check + Playwright-Browsertest)
8. Commit → Push main → cherry-pick auf staging → Push
9. Live-Verifikation beider Domains per curl

## Umsetzung

Wie geplant, ohne inhaltliche Abweichungen. Details:

- `impressum.html`: Untertitel DE/EN auf DDG; EN-Untertitel neu
  „Information according to § 5 DDG (German Digital Services Act)";
  Haftungssatz DE/EN ohne Paragraphen-Nennung (EN: „in accordance with
  general legislation"); Steuer-Sektion (HTML + Divider) und i18n-Keys
  `h3`, `tax-label` (de+en) entfernt; Adresse an 2 Stellen ersetzt.
- `datenschutz.html`: Adresse im Abschnitt „1. Verantwortlicher" ersetzt.
- Der i18n-Loop (`setLang`) hat einen Null-Check, dennoch wurden HTML-Elemente
  und Keys konsistent zusammen entfernt.
- Staging: Fix-Commit `db41669` per `git cherry-pick` in einem separaten
  Worktree auf `staging` angewendet (kein Konflikt) und gepusht — das
  Arbeitsverzeichnis mit WIP-Änderungen blieb unberührt.

## Tests

**Verifikations-greps aus dem Brief** (jeweils direkt nach dem TODO):

| Check | Soll | Ist |
|---|---|---|
| `grep -rn "TMG" . --include="*.html"` | 0 | 0 ✅ |
| `grep -rn "207/5223" .` (HTML) | 0 | 0 ✅ (Treffer nur noch im Brief selbst) |
| `grep -rn "Steuernummer" . --include="*.html"` | 0 | 0 ✅ |
| `grep -rn "Aachener\|52349" . --include="*.html"` | 0 | 0 ✅ |
| `grep -c "Linnich" impressum.html` | ≥ 2 | 2 ✅ |
| `grep -rln "Aachener\|TMG\|207/5223"` (html/js/json) | 0 Dateien | 0 ✅ |

**Statischer i18n↔DOM-Check** (Node-Script): de/en-Key-Mengen identisch,
jeder Key hat ein `t-*`-DOM-Element, keine verwaisten IDs — impressum (12 Keys)
und datenschutz (27 Keys): **PASS**.

**Playwright-Browsertest** (System-Chrome, lokaler Server): 18/19 Checks PASS —
DE-Inhalte (DDG, Adresse 2×, keine Steuernummer, MStV unverändert),
EN-Umschalter (Imprint, DDG-Untertitel, keine Tax Number), Rückschalten DE,
localStorage-Sync, datenschutz DE/EN, index-Links. Einziger FAIL: ein
404-Konsolenfehler für `/favicon.ico` — vorbestehend, nicht durch die
Änderung verursacht (siehe Offene Punkte).

**Live-Verifikation nach Deploy** (curl, beide Domains):

| Check | mscon.one | staging.mscon.one |
|---|---|---|
| impressum DDG ≥ 1 | 1 ✅ | 1 ✅ |
| impressum TMG = 0 | 0 ✅ | 0 ✅ |
| impressum 207/5223 = 0 | 0 ✅ | 0 ✅ |
| impressum Aachener = 0 | 0 ✅ | 0 ✅ |
| impressum Linnich ≥ 1 | 2 ✅ | 2 ✅ |
| datenschutz Aachener = 0 | 0 ✅ | 0 ✅ |
| datenschutz Linnich ≥ 1 | 1 ✅ | 1 ✅ |

## Ergebnisse

- Beide Live-Seiten (Produktion + Staging) zeigen: § 5 DDG, neue Adresse
  Weberstr. 6, 52441 Linnich, keine Steuernummer, MStV-Abschnitt unverändert —
  in DE und EN.
- Betroffene Dateien: `impressum.html`, `datenschutz.html`
- Commits: main `db41669`, staging `2329362`

## Offene Punkte

- **Favicon fehlt**: Browser-Anfrage auf `/favicon.ico` läuft ins Leere
  (404-Konsolenfehler auf jeder Seite). Kosmetisch, ggf. Favicon ergänzen.
- **E-Mail-Obfuskierung** (ehem. TODO #4): bewusst gestrichen; Cloudflare
  Email Protection bleibt aktiv, E-Mail im Quelltext daher nicht im Klartext.
- `IMPRESSUM-UPDATE_1.md` im Root ist durch diesen Bericht inhaltlich
  abgelöst (Datei unangetastet, untracked).
- Uncommitted WIP: `index.html` (SEO-Meta, Self-hosted Fonts), `CLAUDE.md`,
  `assets/fonts/` — separates Vorhaben, nicht Teil dieses Updates.
