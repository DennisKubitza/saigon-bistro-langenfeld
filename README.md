# Saigon Bistro Langenfeld — Website

Statische, mehrseitige Website für das Saigon Bistro (Langenfeld, Rheinland). Reines HTML/CSS/JS
ohne eigenen Build-Schritt und ohne Abhängigkeiten — kann direkt über GitHub Pages gehostet werden.
Ausnahme: `index.html`, `speisekarte.html`, `mittagsangebote.html` und `anfahrt.html` nutzen
Jekyll, das GitHub-Pages-eigene, automatische Build-System, um bearbeitbare Inhalte (Hinweisbox,
Preise, …) aus einfachen Textdateien ins HTML einzusetzen — kein zusätzliches Tooling nötig, siehe
unten.

## Seiten

- `index.html` — Startseite: Hero, Restaurant-Fotos, Google-Rezensionen, Bildergalerie (Slider) der
  Gerichte **und Kontaktbereich** (`#kontakt`)
- `speisekarte.html` — Vollständige, filterbare Speisekarte
- `mittagsangebote.html` — Mittagstisch-Menü (M1–M8)
- `anfahrt.html` — Anfahrtsbeschreibung mit Karte

## Bearbeitbare Inhalte (`content/`-Ordner)

Ein paar Textbausteine auf der Startseite stehen **nicht** im HTML, sondern als einfache Textdateien
im Ordner `content/` — damit sie sich ohne HTML/CSS-Kenntnisse direkt auf GitHub bearbeiten lassen
(Datei öffnen → Stift-Symbol „Edit this file" → Text ändern → committen). Kein JavaScript beteiligt:
GitHub Pages rendert diese Dateien beim Jekyll-Build serverseitig in `index.html` ein (siehe
„Technisch" unten).

| Datei | Wofür | Beispielinhalt |
|---|---|---|
| `content/aktuelles.txt` | Hinweisbox ganz oben auf **allen vier Hauptseiten** (Startseite, Speisekarte, Mittagsmenü, Anfahrt) für kurzfristige Mitteilungen (Betriebsferien, Feiertagsschließung). Eine Datei steuert die Box auf allen vier Seiten gleichzeitig. **Leer = Box wird nirgends angezeigt.** | `Wir sind bis zum 25. Oktober in Betriebsferien. Danach sind wir wieder für Sie da.` |
| `content/abholung.txt` | Abholzeiten im Lieferservice-Banner | `12:15–20:45 Uhr` |
| `content/lieferung.txt` | Lieferzeiten im Lieferservice-Banner | `ab 14:00 Uhr (wochentags), ab 12:15 Uhr (Wochenende)` |
| `content/oeffnungszeiten.txt` | Öffnungszeiten-Banner | `Dienstag – Freitag: 11:00 – 21:00 Uhr · Samstag & Sonntag: 12:00 – 21:00 Uhr · Montag: Ruhetag` |
| `content/google-bewertung.txt` | Google-Bewertung: **Zeile 1** = Punktzahl (mit Punkt, z. B. `4.8`), **Zeile 2** = Anzahl Bewertungen (z. B. `44`). Wird an allen drei Stellen der Startseite verwendet (Trust-Bar oben, Rezensionen-Bereich, `schema.org`-Bewertungsdaten für Suchmaschinen) — eine Änderung hier aktualisiert automatisch alle drei. | `4.8`⏎`44` |

Bei `abholung.txt` / `lieferung.txt` / `oeffnungszeiten.txt` nur den reinen Text eintragen, **ohne**
die fette Überschrift davor (z. B. nur `12:15–20:45 Uhr`, nicht `Abholung: 12:15–20:45 Uhr`) — die
Überschrift steht fest im HTML.

Bei `google-bewertung.txt` bitte die Punktzahl immer mit **Punkt** (`4.9`, nicht `4,9`) eintragen —
das deutsche Komma-Format für die Anzeige wird automatisch daraus erzeugt, während `schema.org`
den Punkt als gültiges Zahlenformat benötigt.


## Speisekarten-Preise (`_data/`-Ordner)

Alle Preise in `speisekarte.html` (151 Stück) und `mittagsangebote.html` (8 Stück) stehen **nicht**
im HTML, sondern in zwei Listen-Dateien:

- `_data/preise_speisekarte.yml`
- `_data/preise_mittagsangebote.yml`

Jede Zeile ist ein **eigenständiger, unabhängiger Preis** — auch wenn mehrere Gerichte gerade
denselben Betrag haben (z. B. viele Gerichte mit „Hühnerfleisch 12,00€"), sind das in der Liste
absichtlich separate Zeilen. Einen Preis für **ein** Gericht zu ändern, ändert also **nie**
versehentlich den Preis eines anderen Gerichts.

```yaml
"1": 3,50€
"2-Huehnerfleisch": 4,50€
"20-A": 12,00€
"20-B": 13,00€
```

**Bearbeiten:** Datei auf GitHub öffnen, nur den Betrag **nach dem Doppelpunkt** ändern (z. B.
`12,00€` → `13,00€`), committen. Die Bezeichnung davor in Anführungszeichen bitte nicht anfassen —
sie verbindet die Zeile mit dem richtigen Gericht auf der Seite (Gerichtnummer wie auf der Karte
gedruckt, bei Gerichten mit Auswahl zusätzlich ein Buchstabe/Kürzel für die jeweilige Variante,
z. B. `20-A` = Gericht 20, Variante A).

**Wird ein ganzes Gericht aus `speisekarte.html` gelöscht:** einfach den ganzen
`<article class="dish">…</article>`-Block löschen. Die übrigen Gerichte sind davon nicht betroffen
— jede Preis-Zeile ist über ihre feste Bezeichnung mit genau einem Gericht verknüpft, nicht über
ihre Position in der Liste. Eine jetzt unbenutzte Zeile in `_data/preise_speisekarte.yml` bleibt
einfach ungenutzt liegen (kein Fehler, keine Auswirkung) und kann bei Gelegenheit mit entfernt
werden, muss aber nicht.

**Fehlt eine Preis-Zeile** (z. B. neues Gericht in der HTML-Datei angelegt, aber die passende
Zeile in `_data/preise_speisekarte.yml` vergessen oder deren Bezeichnung falsch abgetippt), zeigt
die Seite an der Stelle sichtbar **„Preis auf Anfrage"** an, statt einfach leer zu bleiben — das
macht einen vergessenen Preis sofort auf der Live-Seite auffindbar, anstatt lautlos zu verschwinden.


## Struktur

```
website/
├── index.html
├── speisekarte.html
├── mittagsangebote.html
├── anfahrt.html
├── robots.txt        (nur wirksam, wenn am Domain-Root gehostet — siehe oben)
├── content/           (bearbeitbare Textbausteine, siehe „Bearbeitbare Inhalte" oben)
│   ├── aktuelles.txt
│   ├── abholung.txt
│   ├── lieferung.txt
│   ├── oeffnungszeiten.txt
│   └── google-bewertung.txt
├── _data/              (Speisekarten-Preise, siehe „Speisekarten-Preise" oben)
│   ├── preise_speisekarte.yml
│   └── preise_mittagsangebote.yml
├── css/style.css
├── js/main.js
└── images/
    ├── logo.webp
    ├── restaurant-1.webp / restaurant-2.webp   (Innenraum)
    └── dish-*.webp                             (Gerichte, auch im Startseiten-Slider)
```

## Technik

- Kein Framework, kein Build-Schritt — reines HTML/CSS/JS, läuft auf jedem Static-Hosting
  (GitHub Pages, Netlify, eigener Webspace …).
- Schriften: [Fraunces](https://fonts.google.com/specimen/Fraunces) (Überschriften) und
  [Be Vietnam Pro](https://fonts.google.com/specimen/Be+Vietnam+Pro) (Fließtext), von Google Fonts
  eingebunden.
- Kartenanzeige über den offiziellen [OpenStreetMap](https://www.openstreetmap.org)-Iframe-Embed
  (kein API-Key, keine Google-Cookies/-Drittanbieterübertragung — daher auch kein separater
  Datenschutz-Hinweis für die Karte nötig). Koordinaten wurden per OSM-Nominatim für „Zum Stadion 71,
  40764 Langenfeld" ermittelt.
