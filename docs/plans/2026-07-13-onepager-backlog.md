# One-Pager-Backlog: SEO, A11y, DSGVO-Restpunkte

Status: in Arbeit
Branch/Commits: — (übernommen aus alter CLAUDE.md-TODO-Liste)

## Kontext

Die alte (untracked) CLAUDE.md enthielt eine TODO-Liste für den One-Pager.
Bei der CLAUDE.md-Neustrukturierung am 2026-07-13 wurden erledigte Punkte
gestrichen (Impressum/Datenschutz existieren; siehe
`2026-07-13-impressum-ddg-update.md`) und die offenen hierher überführt.

Wichtig: Der volle One-Pager lebt auf `staging`; `main` trägt die
Construction-Page. Die Punkte unten betreffen — sofern nicht anders
vermerkt — den One-Pager auf staging.

## Plan (offene Punkte)

### A — WIP auf main abschließen (SEO-Head Construction-Page)
Lokal uncommitted: SEO-Meta, OG/Twitter-Tags, JSON-LD, Self-hosted Fonts
(`assets/fonts/` liegt bereit, Fonts v17/v38). Testen (Prinzip 3), dann
committen. **Blocker:** `assets/og-image.jpg` (1200×630, dunkler Hintergrund,
Logo + Tagline) existiert nicht — OG-Tags referenzieren es bereits.
JSON-LD-Platzhalter `DEIN-LINKEDIN-HANDLE` füllen.

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

— (noch nicht begonnen; Stand siehe Plan)

## Tests

Je Punkt gemäß CLAUDE.md-Prinzip 3 (Selbst-Test vor Push); für One-Pager
zusätzlich Cursor + Unlock-Button manuell prüfen (kritische Funktionen).

## Ergebnisse

— (offen)

## Offene Punkte

- Platzhalter beim User einholen: LinkedIn-Handle, cal.com-Link
- og-image.jpg erstellen (Tool-Wahl offen: Figma/Canva/Script)
