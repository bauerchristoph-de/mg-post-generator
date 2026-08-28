---
name: ai-post-generator
description: Erstellt fertige Designs im CI des Kunden — Instagram-Posts, Carousels, Stories, Meta-Ads-Statics sowie Flyer, Plakate, Postkarten und Druckdateien — als HTML-Layout mit Export nach PDF/PNG, inklusive Text im Kunden-Wording. IMMER verwenden, wenn eine Grafik, ein Post, ein Carousel, ein Flyer oder eine Werbe-Grafik entstehen oder angepasst werden soll — auch bei Formulierungen wie „mach mir einen Post daraus", „Carousel zu diesem Thema", „Flyer bauen", „Story-Grafik", „Ad-Static", „Text aufs Bild", „nachbauen wie im Original". Beim ersten Einsatz für einen neuen Kunden zuerst das Setup führen (references/setup-design.md). NICHT für Videoschnitt (→ ai-video-cutter).
---

# AI Post- & Design-Generator

Post-Idee rein → fertiges Design plus Text raus. Vom Instagram-Carousel bis zur Druckdatei — gebaut als präzises HTML-Layout, exportiert als PDF/PNG, immer im Design-System des Kunden.

**Grundprinzip: Das Ergebnis muss aussehen, als käme es vom Stamm-Designer der Marke — nicht wie ein Template.** Jede Regel hier stammt aus realen Korrekturschleifen; ihre Verletzung ist sichtbar.

## Zwei Wissensebenen (Architektur — strikt einhalten)

- **Dieses Plugin = Allgemeinwissen.** Gestaltungsregeln, Formate, Technik — zentral gepflegt, kommt per Plugin-Update. Plugin-Dateien nie lokal editieren oder kopieren.
- **Kundenordner = Kundenwissen.** Wird beim Setup angelegt bzw. **mit dem AI Video-Cutter geteilt**, wenn der Kunde beide Pakete hat: `kunden-config.yaml`, `marken-profil.md` und `assets/` sind EIN gemeinsamer Bestand — nie doppelt anlegen. Design-spezifisch kommen `design-system.md`, `design-learnings.md` und `vorlagen/` dazu. Details: `references/learnings-system.md`.

## Update-Check (einmal pro Unterhaltung, still)

1. Installierte Version aus `.claude-plugin/plugin.json` dieses Plugins lesen (Feld `version`) — nie raten, nie hart annehmen.
2. Aktuelle Version per Web-Abruf holen:
   `https://raw.githubusercontent.com/bauerchristoph-de/mg-post-generator/main/.claude-plugin/marketplace.json` → Feld `metadata.version`.
3. Ist die Online-Version höher, dem Nutzer EINEN kurzen Hinweis geben: „Für dein AI-Paket gibt es Version X.Y.Z — bitte einmal in den Einstellungen → Plugins beim Marketplace auf ‚Synchronisieren‘ klicken und danach eine neue Unterhaltung starten.“ Danach normal weiterarbeiten — den Arbeitsfluss nie blockieren, den Hinweis nie wiederholen.
4. Abruf nicht möglich (kein Internetzugriff) → still überspringen, nie erwähnen.

## Pflicht-Lesereihenfolge

1. `kunden-config.yaml` im Kundenordner — CI, Kanäle. **Fehlt sie, zuerst das Setup führen** (`references/setup-design.md`). Nie mit geratenen Farben/Fonts arbeiten.
2. `marken-profil.md` — angelernte Brand Voice (für alle Texte auf Designs). Fehlt es: Marken-Analyse aus dem Setup nachholen.
3. `design-system.md` im Kundenordner — Layout-Master, Farbrollen, Typo-Skala, Referenz-Vorlagen dieses Kunden. **Nie mit einer eigenen Idee starten, wenn eine Vorlage existiert.**
4. `design-learnings.md` — was dieser Kunde dem System schon beigebracht hat. Anwenden statt wiederholen.
5. `references/technik.md` — Rezepte für PDF-Analyse, Druck-Setup, Anschnitt, Messen, Export. Kopieren und anpassen, nicht neu erfinden.
6. `references/formate.md` — Canvas-, Safe-Area- und Ads-Formattabellen.

## Arbeitsweise (verbindlich, mit Freigabe-Schleifen)

Der Kunde gibt pro Schritt frei und korrigiert millimetergenau — der Workflow ist darauf ausgelegt.

| Schritt | Ergebnis |
|---|---|
| 1 Briefing | Format, Medium, Kanal, Anschnitt, Seitenzahl, Ausgabeweg geklärt |
| 2 Sichtung | Vorlage/Design-System gelesen, Assets inventarisiert, Fotos gesichtet |
| 3 Layoutplan | Raster, Farbrollen, Typo-Skala, Elementliste je Seite/Slide |
| 4 Entwurf v1 | gebautes Layout + Kontrollbilder |
| 5 Iterationen | millimetergenaues Feedback einarbeiten |
| 6 Export/Übergabe | Standalone-HTML, PDF, PNGs → `fertig/`; Learnings-Abschluss |

**Wie Anweisungen umgesetzt werden:**
- **Nur ändern, was gesagt wurde.** „Text 3-zeilig" heißt Text kürzen — nicht gleichzeitig Farbe, Abstand oder Schriftgrad anfassen.
- **„Fixieren" heißt fixieren.** Was abgenommen ist, wird später nicht angerührt. **Rückgängig heißt rückgängig** — exakt der vorherige Zustand.
- **Messen und Werte nennen, nicht raten.** „Impressum steht 2,5 mm vom Rand" entscheidet der Kunde in Sekunden.
- **Zweimal dieselbe Anmerkung = strukturelle Ursache suchen** (falsche Referenz, falsches Zentrierverfahren), nicht dritter Versuch mit anderem Wert.
- **Jede Version als neue Nummer** in `projekte/<projekt>/`, Kontrollbilder (PNG je Seite/Slide) immer mitliefern — der Kunde prüft auf dem Handy. Bei Formatvarianten (Endformat/Anschnitt, 1:1/4:5/9:16): jede Änderung in alle Varianten nachziehen.

## Weißraum — die Regel, die am häufigsten reißt

Weißraum ist kein Rest, der übrig bleibt. Er ist ein gesetztes Element.
- **Rand rundum zuerst festlegen**, dann Inhalt hineinbauen. **Links = rechts** — vor der Abgabe nachmessen, nicht ansehen.
- **Oben braucht mehr Luft als man denkt.** Enge oben wirkt sofort billig. Wird unten Platz frei, geht er nach oben — nicht in mehr Inhalt.
- **Druck-Sicherheitszone: mind. 4 mm zur Schnittkante** für alles Lesbare, gerechnet gegen **Endformat minus 2 mm** (Schneidetoleranz). Positionen immer gegen das Endformat prüfen, nie gegen die Anschnittbox.
- **Randabfallende Flächen müssen über die Kante hinauslaufen** — 0 mm Abstand reicht nicht, sonst bleibt eine weiße Linie.
- *Bei Unsicherheit, ob genug Luft ist: Es ist zu wenig Luft.*

## Optische Balance

- **Gruppen bilden:** Zusammengehöriges (Headline + Subline + CTA + QR) erst intern perfekt stellen, dann als EIN Block verschieben.
- **Abstände wiederholen, nicht erfinden:** 2–3 Werte tragen ein ganzes Layout. Vorder-/Rückseite bzw. alle Slides sind EIN System (identische Positionen für wiederkehrende Elemente).
- **Optisch mittig ≠ mathematisch mittig:** Text in Pills sitzt rechnerisch zentriert zu hoch — fester Innenabstand, ~0,2 mm tiefer, am gerenderten Bild kontrollieren.
- **Fotos nach Motiv beschneiden, nicht nach Rahmen** — Rahmen bleibt, Motiv wandert, alle Gesichter im Bild.
- **Hierarchie durch Größe, nicht durch Menge.** Nie alles gleich groß.

## Typografie, Farbe & CI

- **Schriften aus dem Design-System/Original auslesen** (Familie UND Schnitte), nur diese verwenden. Mindestgrößen: Druck 5,5 pt Rechtliches / 8 pt Fließtext; Social 34 px Fließtext / 72 px Hook bei 1080er Canvas. Zeilenabstand explizit setzen. **Text kürzen schlägt Schrift verkleinern.**
- **Nur Farben aus dem Design-System des Kunden** — keine neuen Töne, keine Verläufe, wenn die Marke keine hat. Eine Hauptfarbe + ein Akzent plus Weiß/Dunkel.
- **Logos in der freigegebenen Originaldatei**, nie verzerren, nie umfärben, Schutzraum halten. SVGs ohne Farbdefinition rendern schwarz → dann PNG.
- **Alle Texte auf Designs im Kunden-Wording** aus `marken-profil.md` — Hook-Muster, Wortschatz, No-Go-Wörter gelten auch auf Grafiken.

## Social & Ads → `references/formate.md`

**Organic:** Ein Gedanke pro Bild. Lesehierarchie Hook → Aussage → Marke/CTA. Safe Areas einhalten; Kernaussage im mittigen 1:1-Ausschnitt (Grid-Beschnitt). Carousel: ein Layoutsystem für alle Slides, jeder Slide allein verständlich, 5–8 Slides. Text auf Foto nur mit hergestellter Lesbarkeit (Fläche/Balken/Abdunklung 40–55 %).

**Paid:** Nicht das Organic-Layout recyceln. Je Motiv eigenes Layout pro Seitenverhältnis (1:1, 4:5, ggf. 9:16) — kein Auto-Crop. Hook groß, Marke klein, kein gemalter CTA-Button, Textfläche unter ~20 %. **Pro Variante genau eine Achse ändern** (Hook / Motiv / Farbwelt / Layout), Dateinamen nach Schema — sonst ist die spätere Auswertung wertlos.

## Kontrolle vor Lieferung (hartes Gate)

- Ränder links/rechts/oben/unten je Seite **nachgemessen**, kein Element näher als 4 mm an der Schnittkante (Endformat − 2 mm gerechnet)
- Vorder-/Rückseite bzw. alle Slides deckungsgleich; alle Formatvarianten synchron
- Bündel-Check: Standalone-HTML geöffnet — Bilder da, Schriften eingebettet? Keine Laufzeit-Pfadlogik.
- Texte gegen `marken-profil.md` gelesen (Wording, No-Gos)
- CMYK/ICC macht die Druckerei — immer dazusagen.

## Learnings-Abschluss (Pflicht nach jeder finalen Abnahme)

Korrekturen kategorisieren: kundenspezifisch → `design-learnings.md` bzw. direkt `design-system.md`/Config nachziehen; allgemeingültige Beobachtungen nur im Abschluss-Satz benennen (zentrale Pflege sammelt sie im direkten Kontakt ein). Dem Kunden in einem Satz sagen, was das System für ihn gelernt hat. Details: `references/learnings-system.md`.
