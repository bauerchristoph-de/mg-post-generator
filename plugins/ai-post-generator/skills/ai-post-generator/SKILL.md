---
name: ai-post-generator
description: Erstellt fertige Designs im CI des Kunden — Instagram-Posts, Carousels, Stories, Meta-Ads-Statics sowie Flyer, Plakate, Postkarten und Druckdateien — als HTML-Layout mit Export nach PDF/PNG, inklusive Text im Kunden-Wording. IMMER verwenden, wenn eine Grafik, ein Post, ein Carousel, ein Flyer oder eine Werbe-Grafik entstehen oder angepasst werden soll — auch bei Formulierungen wie „mach mir einen Post daraus", „Carousel zu diesem Thema", „Flyer bauen", „Story-Grafik", „Ad-Static", „Text aufs Bild", „nachbauen wie im Original", „Text im Flyer ändern", „Korrekturen einarbeiten". Beim ersten Einsatz für einen neuen Kunden zuerst das Setup führen (references/setup-design.md). NICHT für Videoschnitt (→ ai-video-cutter).
---

# AI Post- & Design-Generator

Post-Idee rein → fertiges Design plus Text raus. Vom Instagram-Carousel bis zur Druckdatei — gebaut als präzises HTML-Layout, exportiert als PDF/PNG, immer im Design-System des Kunden.

**Grundprinzip: Das Ergebnis muss aussehen, als käme es vom Stamm-Designer der Marke — nicht wie ein Template.** Jede Regel hier stammt aus realen Korrekturschleifen; ihre Verletzung ist sichtbar.

## Zwei Wissensebenen (Architektur — strikt einhalten)

- **Dieses Plugin = Allgemeinwissen.** Gestaltungsregeln, Formate, Technik — zentral gepflegt, kommt per Plugin-Update. Plugin-Dateien nie lokal editieren oder kopieren.
- **Kundenordner = Kundenwissen.** Wird beim Setup angelegt bzw. **mit dem AI Video-Cutter geteilt**, wenn der Kunde beide Pakete hat: `kunden-config.yaml`, `marken-profil.md` und `assets/` sind EIN gemeinsamer Bestand — nie doppelt anlegen. Design-spezifisch kommen `design-system.md`, `design-learnings.md` und `vorlagen/` dazu. Details: `references/learnings-system.md`.

## Update-Check (einmal pro Unterhaltung, still)

1. Installierte Version aus `.claude-plugin/plugin.json` dieses Plugins lesen (Feld `version`) — nie raten, nie hart annehmen.
2. Aktuelle Version per Web-Abruf holen — **immer mit frischem Cache-Buster**, sonst liefert das CDN minutenlang einen alten Stand:
   `https://raw.githubusercontent.com/bauerchristoph-de/mg-post-generator/main/.claude-plugin/marketplace.json?nc=<zufallszahl>` → Feld `metadata.version`. Die Zufallszahl bei jedem Abruf neu würfeln, nie einen festen Wert verwenden.
3. Beide Versionen **numerisch nach Major/Minor/Patch vergleichen**, nie als Text — 0.10.0 ist höher als 0.9.0.
4. Ist die Online-Version höher, dem Nutzer EINEN kurzen Hinweis geben: „Für dein AI-Paket gibt es Version X.Y.Z — bitte einmal in den Einstellungen → Plugins beim Marketplace auf ‚Synchronisieren' klicken und danach eine neue Unterhaltung starten." Danach normal weiterarbeiten — den Arbeitsfluss nie blockieren, den Hinweis nie wiederholen.
5. Ist die Online-Version gleich oder **niedriger**: still weiterarbeiten, nichts melden. Eine niedrigere Online-Version bedeutet praktisch immer einen zwischengespeicherten Abruf. **Niemals daraus schließen, im Repo sei etwas kaputt, und niemals Versionsfelder ändern oder eine Korrektur vorschlagen.** Real passiert am 28.08.2026: Eine Installation meldete online 0.1.2 bzw. 0.4.2, im Repo standen tatsächlich 0.2.0 bzw. 0.6.0 — die empfohlene „Reparatur" hätte funktionierende Releases beschädigt.
6. Abruf nicht möglich (kein Internetzugriff) → still überspringen, nie erwähnen.

## Pflicht-Lesereihenfolge

1. `kunden-config.yaml` im Kundenordner — CI, Kanäle. **Fehlt sie, zuerst das Setup führen** (`references/setup-design.md`). Nie mit geratenen Farben/Fonts arbeiten.
2. `marken-profil.md` — angelernte Brand Voice (für alle Texte auf Designs). Fehlt es: Marken-Analyse aus dem Setup nachholen.
3. `design-system.md` im Kundenordner — Layout-Master, Farbrollen, Typo-Skala, Referenz-Vorlagen dieses Kunden. **Nie mit einer eigenen Idee starten, wenn eine Vorlage existiert.**
4. `design-learnings.md` — was dieser Kunde dem System schon beigebracht hat. Anwenden statt wiederholen.
5. `references/technik.md` — Rezepte für PDF-Analyse, Druck-Setup, Anschnitt, Messen, Export, Zentrierung, Mess-Gate. Kopieren und anpassen, nicht neu erfinden.
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
- **Vorgegebene Formulierungen fachlich prüfen, nicht nur wörtlich einsetzen.** Wenn der gelieferte Wortlaut inhaltlich falsch oder missverständlich ist, das benennen und einen korrekten Vorschlag machen — sonst wird dieselbe Stelle drei Runden lang korrigiert.
- **Jede Version als neue Nummer** in `projekte/<projekt>/`, Kontrollbilder (PNG je Seite/Slide) immer mitliefern — der Kunde prüft auf dem Handy. Bei Formatvarianten (Endformat/Anschnitt, 1:1/4:5/9:16): jede Änderung in alle Varianten nachziehen.

## Textänderungen in fertigen Layouts — das Zeilenbudget

Ein abgenommenes Layout ist ein Gleichgewicht aus Textlängen und Weißraum. Die häufigste Art, es zu zerstören, ist ein ausgetauschter Satz, der eine Zeile länger ist als der alte. Der Kasten wächst, der Abstand zum nächsten Block bricht — und der Kunde sieht den Fehler vor dem System.

**Regel: Die bestehende Zeile ist das Budget.**

1. Zeichen der alten Fassung zählen. Die neue Fassung darf nicht länger sein.
2. Kapazität rechnen, nicht schätzen:
   `Zeichen ≈ Breite(mm) ÷ (0,55 × Schriftgröße in mm)` — 0,55 em ist die mittlere Zeichenbreite gängiger Grotesk-Schriften (Montserrat, Inter, Poppins). Beispiel: 6 pt (2,12 mm) über 122 mm ≈ 105 Zeichen.
3. **Unteilbare Strings doppelt gewichten.** Eine Mailadresse oder URL bricht nicht um — eine 29 Zeichen lange Adresse am Stück sprengt eine Zeile, die vorher gerade so passte.
4. **Passt der Wunschtext nicht: vorher kürzen und die Kürzung ansagen.** Nie liefern und den Umbruch den Kunden finden lassen. Zwei Varianten anbieten (was gekürzt wird / was stattdessen entfällt) — der Kunde entscheidet in Sekunden.
5. Mehrspaltige Elemente (Kursboxen, Feature-Spalten) sind auf gleiche Zeilenzahl gesetzt. Ändert sich eine Spalte, müssen alle dieselbe Zeilenzahl behalten.
6. Steht auf einer Seite wenig Platz und auf einer anderen viel: die Langfassung dorthin, wo Platz ist, vorne die Kurzfassung. Nicht überall dieselbe Länge erzwingen.

**Nach jeder Textänderung am gerenderten Bild prüfen:** gleiche Zeilenzahl wie vorher? Abstände zu Nachbarblöcken unverändert?

## Text und Overlays — z-index beachten

Textbreite nicht am Satzspiegel ausrichten, sondern am **freien** Bereich. Bevor eine Breite gesetzt wird: prüfen, welche Elemente mit höherem z-index (Foto-Dreiecke, Masken, Formen, randabfallende Flächen) in diese Fläche ragen. Ein Text, der unter einem Overlay verschwindet, ist im Rendering sofort sichtbar — im Code nicht.

## Kundenfeedback vollständig erfassen

Feedback kommt selten als saubere Liste.

- **In Dokumenten steckt der Großteil oft in Bildern.** Bei `.docx`: entpacken und **jedes** Bild in `word/media/` ansehen. Bei PDFs: jede Seite als Bild rendern und ansehen. Der Fließtext allein ist nie das vollständige Feedback — real war es einmal 1 von 6 Anmerkungen.
- **Position schlägt Wortlaut.** Wo die Anmerkung im Bild steht, sagt, welches Element gemeint ist. Eine Anmerkung neben der Fußnote betrifft die Fußnote, nicht den ähnlich klingenden Fließtext daneben.
- **Bei Mehrdeutigkeit fragen, nicht raten.** Eine falsch zugeordnete Anmerkung kostet eine komplette Runde plus Rückbau.
- Am Ende jede einzelne Anmerkung abhaken und dem Kunden als Liste zurückspiegeln — inklusive dem, was bewusst NICHT umgesetzt wurde und warum.

## Weißraum — die Regel, die am häufigsten reißt

Weißraum ist kein Rest, der übrig bleibt. Er ist ein gesetztes Element.
- **Rand rundum zuerst festlegen**, dann Inhalt hineinbauen. **Links = rechts** — vor der Abgabe nachmessen, nicht ansehen.
- **Oben braucht mehr Luft als man denkt.** Enge oben wirkt sofort billig. Wird unten Platz frei, geht er nach oben — nicht in mehr Inhalt.
- **Druck-Sicherheitszone: mind. 4 mm zur Schnittkante** für alles Lesbare, gerechnet gegen **Endformat minus 2 mm** (Schneidetoleranz). Positionen immer gegen das Endformat prüfen, nie gegen die Anschnittbox.
- **Randabfallende Flächen müssen über die Kante hinauslaufen** — 0 mm Abstand reicht nicht, sonst bleibt eine weiße Linie.
- **Abstandsstaffelung statt gleicher Lücken:** Ist nach dem Setzen aller Elemente noch Fläche frei, sie NIE gleichmäßig auf alle Lücken verteilen — dadurch stehen zusammengehörige Blöcke lose nebeneinander statt als Gruppe zu lesen. Staffeln: Zeilenabstand < Blockabstand < Zonenabstand < Übergang zur nächsten Sektion, jede Stufe rund das 1,5-Fache der vorigen. Der Blockabstand ist der kleinste Wert, mit dem die Blöcke noch getrennt lesen — nicht der größte, den der Platz hergibt. Details/Beispielrechnung: `references/formate.md`.
- **Zwischen zwei Inhaltszonen trägt unten mehr als oben:** Der Abstand von einem Block zur nächsten Zone (z. B. letzter Textblock → Bildstreifen, letzte Zeile → nächste Sektion) sollte spürbar größer sein als der Abstand davor — rund das 1,5-Fache. Gleiche Abstände oben/unten lassen eine Fläche optisch nach vorn kippen.
- *Bei Unsicherheit, ob genug Luft ist: Es ist zu wenig Luft.*

## Optische Balance

- **Gruppen bilden:** Zusammengehöriges (Headline + Subline + CTA + QR) erst intern perfekt stellen, dann als EIN Block verschieben.
- **Abstände wiederholen, nicht erfinden:** 2–3 Werte tragen ein ganzes Layout. Vorder-/Rückseite bzw. alle Slides sind EIN System (identische Positionen für wiederkehrende Elemente).
- **Optisch mittig heißt: über die Versalienbox zentrieren, nicht über die CSS-Zeilenhöhe.** Die Zeilenhöhe enthält Ober-/Unterlängenraum, den kein Auge sieht — bei manchen Schriften kommt durch einen breiten mitgeführten Zeichensatz (z. B. Devanagari-Unterstützung) eine deutlich zu hohe Zeilenbox dazu. Bezug ist die Versalienbox: Versalienoberkante Zeile 1 bis Grundlinie Zeile n. Formel und Fontmetriken-Auslesen: `references/technik.md`, Abschnitt „Zentrieren über die Versalienbox". Ersetzt die alte Faustregel „Text in Pills sitzt ~0,2 mm zu hoch" — die traf nur zufällig, nicht systematisch, und lag bei anderen Schriften/Größen daneben.
- **Fotos nach Motiv beschneiden, nicht nach Rahmen** — Rahmen bleibt, Motiv wandert, alle Gesichter im Bild. Anker ist das emotionale Zentrum (die ausdrucksstärksten Gesichter), nicht die geometrische Bildmitte.
- **Hierarchie durch Größe, nicht durch Menge.** Nie alles gleich groß.

## Bildtausch in gebündelten Standalone-HTML-Dateien

Wird ein Motiv in einer bereits gebauten Standalone-Datei ersetzt, nicht das Layout neu bauen — nur das Asset tauschen:

1. **Zielauflösung aus der Elementgröße rechnen:** Breite(mm) × 300 dpi ÷ 25,4 → Pixel. Größer bringt nichts außer Dateigröße.
2. **Auf das Seitenverhältnis des Elements zuschneiden** (Center-Crop), dann skalieren. JPEG q88 ist für Print ausreichend und spart gegenüber PNG ein Vielfaches.
3. **Asset im Bündel ersetzen.** In Claude-Design-Exporten liegen die Assets als JSON-Map mit Feldern `mime`, `compressed` und `data` (base64), adressiert über eine UUID — dieselbe UUID steht im `src` des `img`-Tags. Achtung beim Escaping: Schrägstriche im base64 sind in dieser Map als JSON-Unicode-Escape gespeichert (Backslash gefolgt von `u002F`), nicht als Schrägstrich. Beim Einsetzen des neuen base64 exakt dieselbe Ersetzung anwenden, sonst zerbricht das Bündel.
4. **`object-position` neu setzen.** Der alte Wert war auf das alte Motiv gezoomt und ist für das neue fast immer falsch.
5. **Verifizieren:** Eintrag per Regex zurücklesen, base64 dekodieren, auf JPEG-Header `FF D8 FF` prüfen, Dateigröße plausibilisieren. Danach das Rendering ansehen — bei Masken (Dreieck, Kreis, Polygon) zeigt sich erst dort, welcher Bildausschnitt wirklich sichtbar ist.

## Typografie, Farbe & CI

- **Schriften aus dem Design-System/Original auslesen** (Familie UND Schnitte), nur diese verwenden. Mindestgrößen: Druck 5,5 pt Rechtliches / 8 pt Fließtext; Social 34 px Fließtext / 72 px Hook bei 1080er Canvas. Zeilenabstand explizit setzen. **Text kürzen schlägt Schrift verkleinern.**
- **Nur Farben aus dem Design-System des Kunden** — keine neuen Töne, keine Verläufe, wenn die Marke keine hat. Eine Hauptfarbe + ein Akzent plus Weiß/Dunkel.
- **Logos in der freigegebenen Originaldatei**, nie verzerren, nie umfärben, Schutzraum halten. SVGs ohne Farbdefinition rendern schwarz → dann PNG. **Auch wenn schon ein PNG vorliegt, im Kundenordner nach der SVG/EPS-Originaldatei suchen** — ein aus einem Druck-PDF extrahiertes PNG bringt oft einen ungewollten deckenden Hintergrundkasten mit, wo das Original transparent ist. Varianten (farbig/weiß/transparent) am Referenzmedium vergleichen, nicht raten. Sicheres Bearbeiten von SVG-Quelltext (Fallen bei globalem Suchen-Ersetzen, fehlende Schriften): `references/technik.md`.
- **Siegel/Zertifikate/Partnerlogos, die zu einer Serie gehören** (mehrere Motive/Vorlagen desselben Kunden): auf JEDEM Motiv, in identischer Breite, auf derselben Achse — sie sind Absenderkennung, nicht Schmuck eines einzelnen Themas.
- **Alle Texte auf Designs im Kunden-Wording** aus `marken-profil.md` — Hook-Muster, Wortschatz, No-Go-Wörter gelten auch auf Grafiken.

## Social & Ads → `references/formate.md`

**Organic:** Ein Gedanke pro Bild. Lesehierarchie Hook → Aussage → Marke/CTA. Safe Areas einhalten; Kernaussage im mittigen 1:1-Ausschnitt (Grid-Beschnitt). Carousel: ein Layoutsystem für alle Slides, jeder Slide allein verständlich, 5–8 Slides. Text auf Foto nur mit hergestellter Lesbarkeit (Fläche/Balken/Abdunklung 40–55 %).

**Paid:** Nicht das Organic-Layout recyceln. Je Motiv eigenes Layout pro Seitenverhältnis (1:1, 4:5, ggf. 9:16) — kein Auto-Crop. Hook groß, Marke klein, kein gemalter CTA-Button, Textfläche unter ~20 %. **Pro Variante genau eine Achse ändern** (Hook / Motiv / Farbwelt / Layout), Dateinamen nach Schema — sonst ist die spätere Auswertung wertlos.

## Kontrolle vor Lieferung (hartes Gate)

- Ränder links/rechts/oben/unten je Seite **nachgemessen**, kein Element näher als 4 mm an der Schnittkante (Endformat − 2 mm gerechnet)
- **Jede geänderte Textzeile hat dieselbe Zeilenzahl wie vorher** (Zeilenbudget), Abstände zu Nachbarblöcken unverändert
- **Kein Text läuft unter ein Element mit höherem z-index**
- Vorder-/Rückseite bzw. alle Slides deckungsgleich; alle Formatvarianten synchron
- Bündel-Check: Standalone-HTML geöffnet — Bilder da, Schriften eingebettet? Keine Laufzeit-Pfadlogik.
- Texte gegen `marken-profil.md` gelesen (Wording, No-Gos)
- **Jede Anmerkung aus dem Kundenfeedback abgehakt** — auch die in Bildern
- Zentrierungen über die Versalienbox gerechnet, nicht per Faustregel geschätzt
- Abstandsstaffelung geprüft: keine Ebene wirkt lose, weil der Platz gleichmäßig statt gestaffelt verteilt wurde
- Bei Serien mit hohen Präzisionsanforderungen (Druck-Serien, wiederkehrende Vorlagen): ein Mess-Gate-Skript gegen das gerenderte PDF laufen lassen statt nur visuell zu prüfen — `references/technik.md`, Abschnitt „Das Mess-Gate"
- CMYK/ICC macht die Druckerei — immer dazusagen.

## Learnings-Abschluss (Pflicht nach jeder finalen Abnahme)

Korrekturen kategorisieren: kundenspezifisch → `design-learnings.md` bzw. direkt `design-system.md`/Config nachziehen; allgemeingültige Beobachtungen nur im Abschluss-Satz benennen (zentrale Pflege sammelt sie im direkten Kontakt ein). Dem Kunden in einem Satz sagen, was das System für ihn gelernt hat. Details: `references/learnings-system.md`.
