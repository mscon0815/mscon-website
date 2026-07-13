# One-Pager-Backlog: SEO, A11y, DSGVO-Restpunkte

Status: in Arbeit (A ✅ erledigt, B–I offen)
Branch/Commits: main `1b0b06d` + `63dbae6` · staging `b514c34` + `81a18f2`

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

### D — LinkedIn + Kalender-Link (alt #8)
`.cdet`: Rows „LinkedIn" + „Termin buchen" (cal.com). Platzhalter
`DEIN-HANDLE` / `DEIN-CAL-LINK` beim User einholen.

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
| og-image.jpg 200 | ✅ (63695 B, image/jpeg) | — (kein SEO-Head, Punkt B) |
| index: googleapis = 0, JSON-LD = 1, DEIN- = 0 | ✅ | — (Punkt B) |
| impressum/datenschutz googleapis = 0 | ✅ | ✅ |
| fraunces-300.woff2 200 | ✅ | ✅ |

## Ergebnisse

- Punkt A erledigt: Construction-Page auf main hat vollständigen SEO-Head
  (Meta, OG/Twitter mit funktionierendem og-image, JSON-LD ohne Platzhalter),
  Self-hosted Fonts, Mobile-Performance-Tuning.
- Zusatz: Sämtliche Seiten auf **beiden** Branches sind frei von
  Google-Fonts-Requests (DSGVO).
- Commits: main `1b0b06d`, `63dbae6` · staging `b514c34`, `81a18f2`

## Offene Punkte

- B–I aus dem Plan (One-Pager: SEO-Head, DSGVO-Checkbox, LinkedIn/Cal,
  Nav-E-Mail, A11y, Kontrast, Favicon)
- Platzhalter beim User einholen: LinkedIn-Handle, cal.com-Link
- Untracked Font-Formate (eot/svg/ttf/woff) ggf. löschen oder .gitignore
