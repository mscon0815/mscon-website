# One-Pager-Backlog: SEO, A11y, DSGVO-Restpunkte

Status: in Arbeit (A–C, E, F, H, I ✅ · G offen · D zurückgestellt)
Branch/Commits: main `1b0b06d`, `63dbae6`, `24de908` · staging `b514c34`, `81a18f2`, `e36a635`, `6dfb2e1`

## Kontext

Die alte (untracked) CLAUDE.md enthielt eine TODO-Liste für den One-Pager.
Bei der CLAUDE.md-Neustrukturierung am 2026-07-13 wurden erledigte Punkte
gestrichen (Impressum/Datenschutz existieren; siehe
`2026-07-13-impressum-ddg-update.md`) und die offenen hierher überführt.

Wichtig: Der volle One-Pager lebt auf `staging`; `main` trägt die
Construction-Page. Die Punkte unten betreffen — sofern nicht anders
vermerkt — den One-Pager auf staging.

## Plan (offene Punkte)

### A — WIP auf main abschließen (SEO-Head Construction-Page) ✅ ERLEDIGT
Lokal uncommitted: SEO-Meta, OG/Twitter-Tags, JSON-LD, Self-hosted Fonts
(`assets/fonts/` liegt bereit, Fonts v17/v38). Testen (Prinzip 3), dann
committen. **Blocker:** `assets/og-image.jpg` (1200×630, dunkler Hintergrund,
Logo + Tagline) existiert nicht — OG-Tags referenzieren es bereits.
JSON-LD-Platzhalter `DEIN-LINKEDIN-HANDLE` füllen.
→ Umgesetzt am 2026-07-13, siehe „Umsetzung" unten.

### B — SEO-Head auch in den One-Pager (staging)
Derselbe Head-Block (Meta/OG/JSON-LD/@font-face) muss in den One-Pager,
spätestens bevor er auf main geht.

### C — DSGVO-Checkbox im Kontaktformular (alt #7)
In `.cf` vor `.sbtn`:
```html
<div class="ff" style="flex-direction:row;align-items:flex-start;gap:.7rem;">
  <input type="checkbox" id="dsgvo" name="dsgvo" required
    style="margin-top:.2rem;accent-color:var(--gold);cursor:none;flex-shrink:0;">
  <label for="dsgvo" style="font-size:.65rem;color:var(--dim2);line-height:1.6;">
    Ich habe die <a href="/datenschutz.html" style="color:var(--gold);">Datenschutzerklärung</a>
    gelesen und stimme der Verarbeitung meiner Daten zur Kontaktaufnahme zu.
  </label>
</div>
```
i18n-Pflege beachten (Prinzip 5).

### D — LinkedIn + Kalender-Link (alt #8) ⏸ ZURÜCKGESTELLT
User-Entscheidung 2026-07-13: **Terminbuchung entfällt** („Mail muss
reichen"). LinkedIn-Row nur, falls der Handle später nachgeliefert wird.

### E — E-Mail im Nav sichtbar vor Unlock (alt #9)
`mailto:`-Link nach `</ul>` in `nav` (gold, opacity .7→1 on hover).
Hinweis: Alte Vorlage nannte kontakt@mscon.one — aktuelle Adresse ist
schultes@mscon.one.

### F — Logo-img-Attribute + Mobile-Performance (alt #10)
`<img>`: `width/height`, `loading="eager"`, `decoding="async"`.
Three.js: `var isMobile = window.innerWidth < 768;`
Stars `N = isMobile ? 700 : 2200`, Mesh `M = isMobile ? 60 : 180`.

### G — Tastatur-Bedienbarkeit der Karten (alt #11)
Alle 6 `.toggle`: `role="button" tabindex="0"` +
`onkeydown="if(event.key==='Enter'||event.key===' ')tog(N)"`;
CSS: `.toggle:focus-visible{outline:1px solid var(--gold);outline-offset:3px;}`

### H — Kontrast --dim2 (alt #12)
`--dim2: #5a5248` → `#6b6058` (≈4.6:1 auf #070707, WCAG AA).
Danach Design-System-Block in CLAUDE.md aktualisieren.

### I — Favicon ergänzen
`/favicon.ico` fehlt → 404-Konsolenfehler auf jeder Seite
(Befund aus `2026-07-13-impressum-ddg-update.md`).

## Umsetzung

**Punkt A (2026-07-13):**
- JSON-LD bereinigt: `sameAs`-Platzhalter entfernt (LinkedIn-Handle offen,
  → Punkt D), E-Mail auf `schultes@mscon.one` korrigiert (User-Entscheidung).
- `assets/og-image.jpg` generiert: HTML-Template im Design-System
  (Logo invertiert, Fraunces 900, Akzent-Dots gold/rot/olive, Tagline,
  Claim, Domain) → Playwright-Screenshot 2×-DSF → sips-Downscale auf
  exakt 1200×630 JPEG (64 KB).
- Commit `1b0b06d` (main): SEO-Head, Self-hosted Fonts (nur .woff2),
  og-image, Mobile-Partikelreduktion, Logo-img-Attribute, --dim2-Tweak.

**Zusatzbefund + Fix (2026-07-13):** Die Legal-/404-Seiten luden weiterhin
Google Fonts (googleapis-Link) — dieselbe DSGVO-Lücke, die auf index.html
geschlossen wurde. Behoben in `63dbae6` (main): Link durch @font-face-Block
ersetzt (Fraunces 300+700, DM Sans, JB Mono; absolute Pfade wegen
/impressum-Redirect ohne .html). Auf staging via `b514c34`
(Font-Assets) + cherry-pick `81a18f2`.

Anmerkung: Fraunces wird auf der Construction-Page nicht genutzt —
die @font-face-Deklarationen bleiben (kein Netzwerk-Kosten, Parität für
One-Pager). Die nicht referenzierten Font-Formate (eot/svg/ttf/woff)
liegen untracked auf Platte, nur .woff2 ist committed.

**Punkte B, C, E, F, H, I (2026-07-13, zweiter Durchlauf):**
- **I Favicon:** `favicon.ico` (32×32 PNG-in-ICO, dunkles Tile mit Logo,
  Playwright-generiert + Node-ICO-Wrapper) im Root, `<link rel="icon">`
  auf allen Seiten beider Branches. Favicon-404 behoben. (main `24de908`,
  staging cherry-pick `e36a635` — Konflikt in index.html erwartungsgemäß,
  per `--ours` gelöst, One-Pager-Link im Folgecommit)
- **H Kontrast:** `--dim2` → `#6b6058` auf Legal-/404-Seiten (beide
  Branches) und im One-Pager.
- **B SEO-Head One-Pager:** Title (SEO-Variante), Description, robots,
  canonical → mscon.one (verhindert Duplicate-Content der Staging-Domain),
  OG/Twitter, JSON-LD, favicon; googleapis-Link durch @font-face ersetzt
  (Fraunces 300/700/900 + italic 200–300 via 200italic-File, DM Sans,
  JB Mono; absolute Pfade). (staging `6dfb2e1`)
- **C DSGVO-Checkbox:** `#ff-ds` vor dem Submit-Button, eigene CSS-Regeln,
  i18n-Keys `dsgvo_label`/`err_ds` (de+en, Label mit Link via innerHTML),
  Validierung im Submit-Handler (`setErr('ff-ds', …)`), Fehler-Reset bei
  `change`.
- **E E-Mail vor Unlock:** `mailto:schultes@mscon.one` als erster Link in
  der fixen `#legal-bar` (das frühere Nav existiert nicht mehr — Intent
  von Alt-TODO #9 damit erfüllt).
- **F Partikelreduktion:** `isMobile`-Abfrage im One-Pager, Sterne
  700/2200, Mesh 60/180.

## Tests

Je Punkt gemäß CLAUDE.md-Prinzip 3 (Selbst-Test vor Push); für One-Pager
zusätzlich Cursor + Unlock-Button manuell prüfen (kritische Funktionen).

**Punkt A + Zusatzfix (alle PASS):**
- greps: 0× googleapis in *.html, 0× Platzhalter `DEIN-`, 0× kontakt@
- Playwright (System-Chrome, lokaler Server), Construction-Page 13 Checks:
  keine Google-Requests, genutzte Fonts 200, alle @font-face-URLs auflösbar
  (fonts.load), JSON-LD parsebar + korrekte Felder, Meta/OG-Tags, og-image
  200, Canvas + Logo gerendert, keine fehlgeschlagenen Requests außer
  favicon (Punkt I)
- Playwright Legal-/404-Seiten, 12 Checks: 0 Google-Requests, je 3 Fonts
  mit 200, fonts.check für DM Sans/JB Mono/Fraunces true, keine JS-Fehler
- Sichtprüfung Screenshots: og-image.jpg + Construction-Page nach
  Intro-Animation (4,5 s) — Rendering korrekt

**Live-Verifikation (curl):**
| Check | mscon.one | staging.mscon.one |
|---|---|---|
| og-image.jpg 200 | ✅ (63695 B, image/jpeg) | — (Asset auf main) |
| index: googleapis = 0, JSON-LD = 1, DEIN- = 0 | ✅ | ✅ |
| impressum/datenschutz googleapis = 0 | ✅ | ✅ |
| fraunces-300.woff2 200 | ✅ | ✅ |
| favicon.ico 200 | ✅ | ✅ |
| dsgvo-checkbox + isMobile im HTML | — (Construction) | ✅ |
| dim2 = #6b6058 | ✅ | ✅ |

Hinweis: `mailto:`-Link live nicht als Klartext grepbar — Cloudflare
Email Protection (bewusst aktiv, gestrichenes TODO #4) schreibt ihn zu
`/cdn-cgi/l/email-protection` um; im Browser funktional.

**Zweiter Durchlauf — Favicon/dim2 (main, 4 Seiten):** icon-Tag + ico=200,
dim2 korrekt, **0 Konsolenfehler** (Favicon-404 weg) — PASS.

**Zweiter Durchlauf — One-Pager (Playwright, 17 Checks, alle PASS):**
0 Google-Requests, 5 Fonts mit 200, Title/Meta/JSON-LD korrekt,
dim2 neu, mailto in legal-bar, **Cursor folgt Maus**, **Unlock via #ubtn
→ body.open**, **alle 6 Karten → #cwrap.on**, Leer-Submit → Fehler inkl.
DSGVO, Anhaken cleart Fehler, gültiger Submit (Formspree gestubbt,
Payload geprüft), EN/DE-Umschalter inkl. übersetztem Checkbox-Label,
0 fehlgeschlagene Requests, 0 JS-Fehler. Screenshots gesichtet.

## Ergebnisse

- A–C, E, F, H, I erledigt. Beide Domains DSGVO-sauber (keine
  Google-Requests auf keiner Seite), SEO-Head überall, Favicon live
  (404 behoben), Kontrast WCAG AA, One-Pager mit DSGVO-Checkbox (de/en),
  E-Mail vor Unlock erreichbar, Mobile-Partikelreduktion aktiv.
- Kritische Funktionen (Cursor, Unlock, Karten, Kontakt-Reveal, Formular)
  nach allen Änderungen automatisiert getestet — intakt.
- Commits: main `1b0b06d`, `63dbae6`, `24de908` ·
  staging `b514c34`, `81a18f2`, `e36a635`, `6dfb2e1`

## Offene Punkte

- **G (Karten-A11y)**: bewusst offen — wird mit dem geplanten
  Handlungsfelder-Rework gebündelt
- **D**: zurückgestellt (Terminbuchung entfällt; LinkedIn nur bei
  nachgeliefertem Handle)
- **Launch des One-Pagers auf main wartet** auf das inhaltliche Rework
  der Handlungsfelder durch den User
- Untracked Font-Formate (eot/svg/ttf/woff) ggf. löschen oder .gitignore
