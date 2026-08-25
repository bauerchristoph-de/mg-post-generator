# Formate & Safe Areas

## Social — Canvas

| Format | Canvas | Safe Area |
|---|---|---|
| Feed-Post Standard | 1080 × 1350 (4:5) | 80 px rundum |
| Feed quadratisch | 1080 × 1080 | 80 px rundum |
| Carousel-Slide | 1080 × 1350 | 80 px rundum |
| Story / Reel-Cover | 1080 × 1920 | oben 250 px, unten 400 px, seitlich 80 px |

**Grid-Beschnitt:** Instagram zeigt 4:5-Posts im Profilraster als 1:1.
Kernaussage und Marke müssen im mittigen Quadrat stehen.

**Reel-Cover:** muss zusätzlich als 1080 × 1350-Ausschnitt allein funktionieren.

## Social — Typo-Mindestgrößen (1080er Canvas)

| Element | Größe |
|---|---|
| Hook | ≥ 72 px |
| Aussage / Zwischenzeile | ≥ 48 px |
| Fließtext | ≥ 34 px |
| Kleingedrucktes | ≥ 26 px |

Textmenge: Feed-Post max. ~12 Wörter im Bild, Story max. ~20.

## Carousel

- 5–8 Slides. Ein Layoutsystem für alle: gleiche Ränder, gleiche Hook-Höhe,
  gleiche Position von Seitenzahl und Marke.
- Slide 1 verkauft (Hook + Nutzen), letzte Slide hat den CTA.
- Jeder Slide muss allein verständlich sein — Leute steigen mittig ein.
- Swipe-Indikator auf Slide 1 und den mittleren, nicht auf der letzten.

---

## Ads — Formate

Je Motiv mindestens **1:1 (1080 × 1080)** und **4:5 (1080 × 1350)**, bei
Story-/Reels-Placement zusätzlich **9:16 (1080 × 1920)**. Jedes
Seitenverhältnis bekommt ein eigenes Layout — kein Auto-Crop durch Meta.

Ränder großzügiger als Organic: **100 px**.

## Ads — Gestaltung

- Hook oben, groß, kontrastreich. Im Zweifel größer.
- Nutzen statt Marke: Logo klein und nachgeordnet.
- Kein gemalter CTA-Button — Meta liefert den echten. Ausnahme: der CTA ist
  Teil der Aussage („Nur 12 Plätze").
- Textfläche insgesamt unter ~20 % der Bildfläche.
- Preis / Termin / Rabatt bekommen ein eigenes Element (Badge, Pill), nicht
  den Fließtext.

## Ads — Testachsen

Pro Variante genau **eine** Achse ändern:

| Achse | Beispiele |
|---|---|
| Hook-Text | Frage vs. Aussage vs. Zahl |
| Motiv | Person in Aktion vs. Gruppe vs. Detail |
| Farbwelt | Hauptfarbe-dominant vs. Akzent-dominant |
| Layout | Text auf Fläche vs. Text auf Foto |

## Ads — Benennung

```
<Kunde>_<Kampagne>_<Familie>_<Variante>_<Format>.png
MusterGmbH_Fruehjahrsaktion_HookFrage_A_1080x1350.png
```

**Familie** = gemeinsames Grundlayout, **Variante** = die eine geänderte Achse.
Ohne diese Systematik ist die spätere Ad-Auswertung wertlos (Testachsen
nicht mehr zuordenbar).

---

## Druck — Standardformate

| Erzeugnis | Endformat | Anschnitt |
|---|---|---|
| Flyer quadratisch | 150 × 150 mm | +3 mm rundum → 156 × 156 |
| Flyer DIN lang | 99 × 210 mm | +3 mm |
| Postkarte A6 | 105 × 148 mm | +3 mm |
| Plakat A3 | 297 × 420 mm | +3 mm |

Sicherheitszone für Lesbares: **4 mm** — gerechnet gegen Endformat **minus
2 mm** Schneidetoleranz.

---

## Weißraum & Zuordnung (Pflichtregeln, Learnings 25.08.2026 KiSpo-Flyer)

**1. Weißraum wird gemessen, nicht geschätzt — auch INNERHALB von Containern.**
Ein Badge/Pill, das über eine Boxkante ragt, braucht Luft zum nächsten
Element *unter* der Kante (Icon, Text), nicht nur zur Kante selbst.
Richtwert: ≥ 3 mm (Druck) / ≥ 24 px (1080er Canvas) zwischen Badge-Unterkante
und nächstem Inhalt. 1 mm Abstand liest sich als Kollision.

**2. Zuordnung (Gesetz der Nähe): Kleintexte gehören zu ihrem Bezugselement.**
Fußnoten, Disclaimer, Sternchen-Texte müssen dem Element, auf das sie sich
beziehen, eindeutig zugeordnet sein: Abstand zum Bezugselement DEUTLICH
kleiner als zu allem anderen (Faustregel ≥ Faktor 3 — z. B. 1,2 mm zur
Preis-Box, 4 mm zum nächsten fremden Block). Ein Kleintext, der frei
zwischen zwei Blöcken schwebt, wirkt unaufgeräumt.

**3. Nachrücken nach Element-Entfernung.**
Wird ein Element entfernt (Logo-Zeile, Bild, Absatz), wird die Fläche neu
verteilt — verbleibende Elemente nachrücken/zentrieren, Weißraum bewusst
oben UND unten verteilen. Nie die Lücke einfach stehen lassen: dann ist der
Block oben gequetscht und unten leer.

**4. Boxen atmen unten.**
Zwischen letzter Textzeile und Box-Unterkante braucht es spürbar Innen-Luft:
≥ 5 mm im Druck / ≥ 40 px auf 1080er Canvas (mindestens 1,5× Zeilenhöhe).
Konsistenz im Dokument: Alle Boxen gleich luftig — die luftigste Box setzt
den Standard, nicht die engste (Vorder- und Rückseite vergleichen).

Diese vier Prüfungen gehören in JEDEN Render-Check vor Abgabe
(siehe technik.md „Messen statt schätzen").
