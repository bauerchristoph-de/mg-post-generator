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
| Text in Pill/Button rutscht hoch | `justify-content: center` | fester `padding-top` in mm, besser: über die Versalienbox positionieren (siehe unten) |
| Bild fehlt in gebündelter Datei | JS setzt `src` zur Laufzeit | Pfad statisch ins Markup |
| Schrift fällt auf Fallback zurück | Google-Fonts-Link statt Einbettung | Standalone-Bundle nutzen |

### Vierfach-Rendering gegen Rundungsfehler

Bei Toleranzen unter ca. 0,3 mm reicht normales Chromium-Rendering nicht:
Chromium rundet jede Layout-Position auf ein volles CSS-Pixel (0,265 mm bei
96 dpi) — bei mm-genauen Druckvorgaben der größte Fehler im System.

**Fix:** die Seite in vierfacher Größe bauen und drucken (`@page { size:
<Breite×4>mm <Höhe×4>mm; }`, alle mm-Werte im Layout ×4), danach das PDF
**verlustfrei als Vektor** — nicht rasterisiert — auf die Zielgröße
herunterskalieren: eine neue PDF-Seite in der Zielgröße anlegen und das
Quell-PDF per `show_pdf_page(ziel_rect, quelle, 0, clip=Rect(0,0,B×4×PT,
H×4×PT))` (PyMuPDF) hineinskalieren. Restfehler sinkt auf ~0,07 mm.

**Nie die Seitenbox eines bereits gerenderten PDFs verschieben**
(`set_mediabox`/Crop-Box auf die Zielgröße setzen). PDF-Koordinaten zählen
von unten links; ist das Rendering auch nur geringfügig größer als die
Zielgröße, bleibt beim reinen Verschieben der leere Papierrand des
Renderings sichtbar im Ausschnitt — in der Praxis: ein weißer Streifen an
einer Schnittkante, der bei flüchtiger Prüfung leicht übersehen wird, weil
er nur an einer von vier Kanten auftritt. Immer eine frische Zielseite
anlegen und hineinskalieren, nie die vorhandene Seite zurechtschneiden.

---

## Zentrieren über die Versalienbox

CSS-Zeilenhöhe ist die falsche Bezugsgröße für optische Zentrierung (Icon +
Text, Text in Pills/Buttons, mehrzeilige Kopfzeilen). Sie enthält
Ober-/Unterlängenraum, den kein Auge sieht, und ist je nach Schriftfamilie
unterschiedlich groß — manche Schriften führen einen breiten
Unicode-/Sprachumfang mit (z. B. Devanagari-Unterstützung), der die
Zeilenbox deutlich über die sichtbaren Glyphen hinaus aufbläht. Eine
Korrektur wie „~0,2 mm tiefer" trifft dann nur zufällig, nicht systematisch,
und liegt bei anderer Schriftgröße oder Zeilenzahl wieder daneben.

**Richtige Bezugsgröße ist die Versalienbox:** von der Versalienoberkante
(cap-height) der ersten Zeile bis zur Grundlinie der letzten Zeile.

1. Fontmetriken direkt aus der TTF/OTF lesen, nicht schätzen — z. B. mit
   `fontTools`: `TTFont` laden, `getGlyphSet()`, mit einer `BoundsPen` die
   Bounding-Box eines Großbuchstaben (z. B. „H") vermessen → cap-height in
   em. Ascender/Descender stehen in den `hhea`-/`OS/2`-Tabellen, ebenfalls
   in em.
2. Jede Textposition wird über die **Grundlinie der ersten Zeile** gesetzt,
   nie über `top`/Box-Padding: `top_css = grundlinie − ascender·fs`.
3. Umrechnung zwischen gewünschter optischer Mitte und der zu setzenden
   Grundlinie (fs = Schriftgröße, n = Zeilenzahl, cap = cap-height in em,
   Zeilenabstand in derselben Einheit wie fs):

   ```
   Mitte       = Grundlinie₁ − cap·fs/2 + (n−1)·Zeilenabstand/2
   Grundlinie₁ = Mitte + (cap·fs − (n−1)·Zeilenabstand)/2
   ```

Damit lässt sich eine vorgegebene optische Mitte (Icon-Mittelpunkt,
Pill-Mitte, Kreis-Mittelpunkt) exakt in eine CSS-Position zurückrechnen,
statt sie am gerenderten Bild in mehreren Runden anzunähern.

---

## SVG sicher bearbeiten

Zwei Fallen bei SVGs aus Kundendateien (Logos, Siegel, Zertifikate), die vor
dem Einbetten per Skript angepasst werden:

1. **Nie global im SVG-Quelltext suchen-und-ersetzen** — z. B. alle
   `width=`/`height=`-Attribute im gesamten Dokument ersetzen, um die
   Gesamtgröße zu setzen. Trifft das auch interne Formen (`<rect>`,
   `<path>`) mit, verschwinden Flächen, die sichtbar sein sollten — z. B.
   der weiße Hintergrund eines Siegels; übrig bleibt nur die Kontur.
   **Nur das öffnende `<svg ...>`-Tag selbst anfassen**, per Regex eng
   verankert (`^<svg\b[^>]*>`), alles danach unverändert lassen.
2. **Fehlt dem Renderer eine im SVG referenzierte Schrift**, fällt der Text
   lautlos auf eine System-Ersatzschrift zurück — sichtbar oft nur bei
   Mikroschrift/Kleingedrucktem innerhalb eines Logos, leicht zu übersehen.
   Fix: `font-family` im SVG-Quelltext vor dem Einbetten auf eine sicher
   verfügbare/bereits eingebettete Schrift umschreiben, oder die
   Originalschrift direkt in einen `<style>`-Block im SVG einbetten.

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
- Text-Position innerhalb von Buttons (oben/unten getrennt) — besser noch:
  über die Versalienbox gerechnet statt nachträglich am Bild korrigiert
  (siehe oben)
- Deckungsgleichheit Vorder- / Rückseite

Für Detailkontrolle: Seite mit `snapshot_element` in 4× aufnehmen, den
relevanten Ausschnitt per Canvas herausschneiden und vergrößert ansehen.

---

## Das Mess-Gate — automatisiert prüfen statt ansehen

„Messen statt schätzen" oben heißt: von Hand am gerenderten Bild nachmessen.
Bei Serien und wiederkehrenden Vorlagen (mehrere Motive derselben Kampagne,
mehrere Varianten eines Formats, ein Kunde mit laufend neuen Ausgaben
derselben Vorlage) lohnt sich ein Schritt weiter: ein Skript, das das
fertige PDF rastert (z. B. PyMuPDF, 600 dpi in ein numpy-Array) und die
Kriterien programmatisch prüft, statt sie bei jeder Iteration erneut
visuell zu beurteilen. Ein Befund heißt: nicht liefern.

Geprüfte Kriterien (Ausgangsliste, je nach Auftrag ergänzen):

| Prüfung | Kriterium |
|---|---|
| Seitenformat | exakte Maße ± 0,02 mm |
| Randabfall | Anschnittzone rundum zu > 99,8 % bedruckt (keine weiße Kante) |
| Satzrand | keine Textzeile ≥ 5 pt über die Satzbreite hinaus (Mikroschrift in Logos ausnehmen) |
| Sicherheitszone | nichts Lesbares näher als die Sicherheitszone an der Schnittkante |
| Zentrierung | gemessene Text-/Ankermitte gegen Soll-Mitte, Toleranz ~0,25 mm (Formeln oben) |
| Zeilenlage | Grundlinie aus Ober- und Unterkante des Textes identisch, Toleranz ~0,3 mm |
| Kontrast | helle Schrift auf Foto: 98-Perzentil-Luminanz des Untergrunds ≤ 150 |
| Kollision | keine zwei Zeilen/Elemente mit > 60 % vertikaler Überdeckung |

Praktische Hinweise:

- **Kontrast-Messung:** die Textmaske vor dem Ausschließen aus der
  Hintergrund-Stichprobe leicht dilatieren (z. B.
  `scipy.ndimage.maximum_filter`) — sonst verfälschen anti-aliaste
  Glyphenkanten die gemessene Helligkeit. Nur auf Flächen mit
  Helligkeits-Streuung anwenden (Foto), nicht auf einfarbige Flächen (Pill,
  Flat-Hintergrund) — dort schlägt die Prüfung sonst grundlos an.
- **Satzrand-Prüfung** auf Fließtext-Größen (≥ 5 pt) begrenzen — sonst
  meldet sie legitime Mikroschrift innerhalb von Logos/Siegeln als Fehler.
- **Ein Kriterium, das falsch anschlägt, wird präzisiert — nie
  stillschweigend abgeschaltet.** Ein Fehlalarm ist ein Hinweis, dass die
  Prüfbedingung noch zu grob ist (z. B. fehlende Ausnahme für Mikroschrift),
  nicht, dass die Prüfung überflüssig ist.
- Für ein Einzelstück lohnt sich das Skript oft nicht; ab der zweiten
  Vorlage/Variante eines Kunden schon, weil dieselben Prüfungen sonst bei
  jeder Iteration erneut von Hand laufen — und weil Befunde dann messbar
  sind, statt von der Tagesform der visuellen Prüfung abzuhängen.
