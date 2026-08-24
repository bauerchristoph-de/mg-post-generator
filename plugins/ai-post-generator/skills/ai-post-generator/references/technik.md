# Technik-Rezepte

## Original-PDF auslesen

**Text:** pdf-parse (Browser-Build, gepinnt) im Skript laden, `getText()`.

**Schriften und Schnitte** (das ist der Weg, der funktioniert):
pdfjs-dist laden, pro Seite `getOperatorList()`, alle `OPS.setFont`-Aufrufe
sammeln und über `page.commonObjs` auflösen. Liefert Namen wie
`Montserrat-ExtraBoldItalic` inklusive Subset-Präfix.

**Achtung:** `getScreenshot()` von pdf-parse läuft bei großen Druck-PDFs ins
Timeout. Wenn ein visueller Eindruck gebraucht wird: den Kunden um einen Screenshot bitten — schneller als jeder Renderversuch.

**Farben** aus einem Screenshot pipettieren: Bild in Canvas zeichnen,
Pixelwerte lesen.

---

## Druck-Layout aufsetzen

- Starter `doc_page.js` kopieren, im Template nach `</helmet>` referenzieren.
- Seitengröße explizit pinnen: `<doc-page width="150mm" height="150mm">`.
- Eine `<section class="page" data-screen-label="Vorderseite">` pro Seite.
- Alles absolut in **mm** positionieren, keine Viewport-Einheiten.
- `data-screen-label` setzen — dadurch sind Kommentare eindeutig zuordenbar.

### Anschnitt-Variante ableiten

Aus der Endformat-Datei erzeugen: Seitenbox auf Endformat + 2×3 mm setzen und
den kompletten Seiteninhalt in einen Wrapper mit `left: -3mm; top: -3mm`
legen. Dadurch bleibt jede Koordinate im Layout identisch und randabfallende
Flächen laufen automatisch in den Anschnitt.

### Was im Druck anders rendert als am Bildschirm

| Problem | Ursache | Lösung |
|---|---|---|
| Text in Pill/Button rutscht hoch | `justify-content: center` | fester `padding-top` in mm |
| Bild fehlt in gebündelter Datei | JS setzt `src` zur Laufzeit | Pfad statisch ins Markup |
| Schrift fällt auf Fallback zurück | Google-Fonts-Link statt Einbettung | Standalone-Bundle nutzen |

---

## Auslieferung

**Standalone-HTML** (der Standard-Weg):
`<template id="__bundler_thumbnail">` mit einfachem SVG einfügen, Asset-Pfade
relativ zum Bündelziel korrigieren, dann bündeln. Danach **immer** öffnen und
prüfen: Bilder sichtbar? `@font-face`-Blöcke mit base64-Daten vorhanden?

Druckweg des Kunden: Datei speichern → Doppelklick → Strg+P → als PDF sichern,
**Skalierung 100 %**. Ergibt Vektor-Text.

**PDF direkt aus Bildern** (nur als Fallback, ergibt Pixel-PDF):
Seiten als PNG mit `snapshot_element` (scale 3 ≈ 288 dpi bei 150 mm), dann
jsPDF mit `format: [breite, höhe]` in mm, je Seite `addImage(..., 0, 0, b, h)`.

**Was ich nicht kann:** CMYK-Konvertierung und ICC-Profile. Das macht die
Druckerei. Immer dazusagen, wenn eine Druckdatei rausgeht.

---

## Messen statt schätzen

Vor der Abgabe im gerenderten Dokument nachmessen — Element-Rechtecke gegen
das Seiten-Rechteck rechnen und in mm umrechnen (`px pro mm` = Seitenbreite in
px / Seitenbreite in mm). Immer prüfen:

- Rand links / rechts / oben / unten je Seite
- Abstand kritischer Elemente zur Schnittkante (bei Endformat **minus 2 mm**
  rechnen für Schneidetoleranz)
- Text-Position innerhalb von Buttons (oben/unten getrennt)
- Deckungsgleichheit Vorder- / Rückseite

Für Detailkontrolle: Seite mit `snapshot_element` in 4× aufnehmen, den
relevanten Ausschnitt per Canvas herausschneiden und vergrößert ansehen.
