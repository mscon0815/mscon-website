# Tagline: „IT-Architektur" → „IT-Roadmap"

Status: umgesetzt ✅
Branch/Commits: `fix/tagline-it-roadmap` (PR #1 → staging squash-merged `9418752`) · main direkt-Commit `80c2112`
Projekt-ID: `PRJ-005` · Portfolio: `mscon/portfolio#5` (Forgejo, `forge.hs.home`)

## Kontext

Positionierungs-Facette ändern: Der sichtbare Tagline auf dem „Ladebildschirm" (Construction-Page `mscon.one`) und auf dem One-Pager (`staging.mscon.one`) soll von „IT-Architektur" auf „IT-Roadmap" wechseln. SEO-Meta, Footer-Copyright und Service-Katalog („IT-Architektur & Design" als Angebot) bleiben **bewusst** unverändert — ein breiterer Rebranding-Scope ist ein separates Vorhaben mit eigener Entscheidung.

Entscheidungen (grill-with-docs 2026-10-15):
- **Q1a** streng minimal: nur sichtbarer Tagline (SEO-Keywords bleiben wie sie sind — „IT-Architektur" hat höheres Suchvolumen; SEO-Shift wäre eigener bewusster Change)
- **Q2b** Promote-Flow: staging zuerst, nach Verifikation main

## Stakeholder

| Rolle | Vertreter | Zeitpunkt der Einbindung | Was sie bekommen |
|---|---|---|---|
| **Owner / Business-Entscheid** | Mark (schultes@mscon.one) | vor dem Change | Scope-Entscheidung (dieses Dokument §Kontext); Freigabe-Punkt zwischen staging-OK und main-Push |
| **Agent / Executor** | Claude Code | während Umsetzung | diese Plan-Datei, CLAUDE.md des Projekts, Branch-Konvention |
| **Reviewer / QA** | Mark (Doppelrolle, Ein-Mann-Betrieb) | nach jedem Deploy | staging-URL + grep-Konsistenz-Report + Browser-Augen-Check |
| **Platform / Deploy** | Cloudflare Pages | automatisch bei Push | Build-Log → Status in Cloudflare-UI; keine manuelle Aktion nötig |
| **Portfolio-Sicht** | Forgejo-Issue `mscon/portfolio#5` | am Ende | Change-Kommentar mit Branch, Commits, Live-URLs |
| **SEO / Marketing** | — (hier aktiv **nicht** betroffen, dokumentiert) | — | Hinweis: Keywords unverändert — nur sichtbarer Text ändert sich |
| **Legal / Compliance** | — (nicht betroffen) | — | Firmenbezeichnung in Impressum/Footer bleibt „IT-Architektur & Digitale Transformation" |
| **End-User / Besucher** | Website-Besucher | nach main-Deploy | neuer Tagline auf `mscon.one` |
| **DSGVO / Datenschutz** | — (nicht betroffen) | — | keine Tracking-/Cookie-Änderung, keine neuen externen Ressourcen |

## Plan

1. Plan-Datei schreiben (dieses Dokument). ✓
2. Branch `fix/tagline-it-roadmap` von `origin/staging` auschecken.
3. In `index.html` auf `staging` ersetzen (3 Zeilen):
   - DOM: `<div id="tagline" data-i18n="tagline">IT-Architektur…</div>` → `IT-Roadmap…`
   - JS `i18n.de.tagline`: `'IT-Architektur …'` → `'IT-Roadmap …'`
   - JS `i18n.en.tagline`: `'IT Architecture …'` → `'IT Roadmap …'`
4. Selbst-Test:
   - `grep -n "IT-Architektur\|IT Architecture" index.html` → außer **unberührten** Treffern (SEO, Footer, Service-Katalog) keine Überraschungen
   - `grep -n "IT-Roadmap\|IT Roadmap" index.html` → genau 3 Treffer (DOM + JS DE + JS EN)
   - `python3 -m http.server 8000`, Browser-Check `http://localhost:8000` DE und EN (Sprach-Toggle)
5. Commit (Conventional, **ohne** Co-Authored-By — CLAUDE.md §7), Push `fix/tagline-it-roadmap`.
6. GitHub-PR → merge `staging`. Cloudflare auto-deploy abwarten (~1–3 min).
7. Live-Check: `curl -s https://staging.mscon.one | grep tagline`; Browser visuell.
8. **Freigabe-Punkt bei Mark**: staging OK → GO für main?
9. In `index.html` auf `main` dieselbe Änderung applizieren (dort nur 2 Zeilen: JS DE + JS EN; kein statisches DOM-Element).
10. Commit auf `main`, Push, auto-deploy, Live-Check `mscon.one`.
11. Status in diesem File auf `umgesetzt ✅`; Kommentar in Forgejo-Issue `mscon/portfolio#5`.

## Umsetzung

1. ✅ Plan-Datei auf `fix/tagline-it-roadmap` angelegt
2. ✅ Branch `fix/tagline-it-roadmap` von `origin/staging` erstellt
3. ✅ `index.html` — 3 Zeilen auf `staging`:
   - DOM Line 357 (sichtbarer Fallback-Text)
   - JS `i18n.de.tagline` Line 944
   - JS `i18n.en.tagline` Line 970
4. ✅ Lokaler `python3 -m http.server 8765`, grep-Tests + i18n-Konsistenz-Check (45 Keys DE = 45 Keys EN)
5. ✅ Commit `9418752` (Conventional, ohne Co-Authored-By — Projekt-Konvention)
6. ✅ PR #1 → `staging` squash-merged, Branch gelöscht
7. ✅ Cloudflare Pages auto-deploy → `staging.mscon.one` → **17:14 UTC live** (cf-ray `a4b07570…-FRA`)
8. ✅ Owner-Freigabepunkt (visueller Check staging) — bestätigt
9. ✅ Checkout `main`, 2 Zeilen in `index.html` ersetzt (nur JS-i18n DE+EN; main hat kein statisches DOM-Element für tagline)
10. ✅ Commit `80c2112` direkt auf `main` (Fast-Forward Push)
11. ✅ Cloudflare auto-deploy → `mscon.one` → **17:36 UTC live** (cf-ray `a4b09669…-FRA`, ~20 s nach Push)
12. ✅ Portfolio-Issue-Kommentar `mscon/portfolio#5` (comment-ID 28)
13. ✅ Status in dieser Datei auf „umgesetzt ✅"

## Tests

| Prüfung | Befehl / Methode | Ziel-Ergebnis |
|---|---|---|
| grep-Konsistenz staging | `grep -c "IT-Roadmap\|IT Roadmap" index.html` | 3 |
| grep SEO unverändert | `grep -c "IT-Architektur\|IT Architecture" index.html` | ≥ 10 (unberührte SEO-Vorkommen) |
| i18n DE-Keys = EN-Keys | manueller Vergleich der Keys in `i18n.de` und `i18n.en` | identisch |
| Lokal Browser DE | `http://localhost:8000`, Tagline sichtbar | „IT-Roadmap · Digitale Transformation" |
| Lokal Browser EN | Sprach-Toggle `EN` | „IT Roadmap · Digital Transformation" |
| staging live | `curl -s https://staging.mscon.one \| grep -oE 'IT-?Roadmap[^<]{0,50}' \| head` | 1 Treffer |
| main live | `curl -s https://mscon.one \| grep -oE 'tagline[^<]{0,200}'` | neue Werte in JS |

## Ergebnisse

### Dateien & Commits
- `index.html` auf `staging` (3 Zeilen) · Commit `9418752` · PR #1
- `index.html` auf `main` (2 Zeilen) · Commit `80c2112`
- `docs/plans/2026-10-15-tagline-it-roadmap.md` · Commit `9418752` (im Change-Branch), nachgezogen in `main` als separater Doku-Commit

### Live-Verifikation (gecurlt, nicht nur gepingt)
- `staging.mscon.one` — `tagline: 'IT-Roadmap · Digitale Transformation'` (DE) · `'IT Roadmap · Digital Transformation'` (EN)
- `mscon.one` — identisch; Deploy ~20 s nach Push (schneller als „bis zu 3 min" aus CLAUDE.md angenommen)

### Beobachtungen für den Playbook-Destillat
1. **PR auf staging mit squash-merge, main dagegen direkt-Commit** — Grund: Construction-HTML (`main`) und One-Pager-HTML (`staging`) divergieren stark; ein Branch-Merge würde nicht sauber laufen. Pattern: *Cherry-pick der Semantik, nicht der Datei.*
2. **grep-Baseline ≥ 10 unberührt** war der entscheidende Health-Check: ohne diesen hätte man übersehen können, dass Services/Footer/SEO gewollt stehen bleiben.
3. **i18n DE-/EN-Key-Mengen-Vergleich** (CLAUDE.md §3) ist der automatische Fehlfang gegen die häufigste Fehlerquelle in diesem Projekt — gehört in jedes Textänderungs-Playbook.
4. **Cloudflare-Deploy-Polling mit präzisem grep-Muster** (`id="tagline"` statt nur `tagline` — CSS-Zeile fängt sonst immer vorab) — Learning in diesem Lauf aufgenommen.
5. **Attribution-Zeile projektseitig AUS** (CLAUDE.md §7) — gegensätzlich zu `homeserver-stack`, wo die Session Co-Authored-By verlangt. Pro Projekt entscheiden, nicht pauschal.
6. **Mehrere laufende Sessions mit verschiedenen Scopes:** Token für Portfolio-Issue-Kommentar war `write:issue`-only, serverseitig in mode-600-Datei, nur temporär — nie durch stdout. Pattern für API-Operationen aus dem Chat.

### Scope bewusst zurückgestellt (eigener Change, falls gewünscht)
- SEO-Keyword-Shift („IT-Architektur" → „IT-Roadmap" in `<title>`, `<meta description>`, og/twitter, Schema.org `keywords`+`description`) — Business-Entscheidung mit SEO-Impact
- Service-Katalog-Umbenennung („IT-Architektur & Design" als Bento-Card)
- Firmenbezeichnung in Impressum/404-Footer („IT-Architektur & Digitale Transformation") — rechtlich relevant
- `CLAUDE.md` dieses Projekts — reine Doku-Konsistenz

## Rollback

- `git revert <sha>` auf dem betroffenen Branch → push → Cloudflare deployt zurück. Blast-Radius: eine Textzeile pro Branch.
- Portfolio-Issue-Kommentar aktualisieren, Status in dieser Datei auf `verworfen ❌` mit Begründung.
