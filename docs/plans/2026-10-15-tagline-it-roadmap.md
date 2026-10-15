# Tagline: „IT-Architektur" → „IT-Roadmap"

Status: in Arbeit
Branch/Commits: `fix/tagline-it-roadmap` (von `staging`) · tbd
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

_(wird während der Ausführung fortgeschrieben)_

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

_(wird nach Live-Verifikation gefüllt — Commits, Live-URLs, Beobachtungen, Lessons Learned für Playbook-Destillation)_

## Rollback

- `git revert <sha>` auf dem betroffenen Branch → push → Cloudflare deployt zurück. Blast-Radius: eine Textzeile pro Branch.
- Portfolio-Issue-Kommentar aktualisieren, Status in dieser Datei auf `verworfen ❌` mit Begründung.
