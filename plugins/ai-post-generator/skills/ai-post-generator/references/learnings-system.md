# Learnings-System & Kundenordner

## Geteilter Kundenordner (Interop mit AI Video-Cutter)

Beide Pakete arbeiten im SELBEN Arbeitsordner des Kunden. Gemeinsamer Bestand (nur einmal, egal welches Paket zuerst da war): `kunden-config.yaml` · `marken-profil.md` · `assets/` (Logos, Fonts). Design-spezifisch dazu:

```
<kundenordner>/
├── kunden-config.yaml      # GETEILT: CI, Kanäle, Intensität
├── marken-profil.md        # GETEILT: angelernte Brand Voice
├── assets/                 # GETEILT: Logos, Fonts
├── design-system.md        # Design: Layout-Master, Farbrollen, Typo-Skala (Setup Schritt 2)
├── design-learnings.md     # Design: kundenspezifische Learnings
├── vorlagen/               # Design: Referenz-Vorlagen des Kunden („die Wahrheit")
├── projekte/<projekt>/     # Design: Briefing, Sichtung, Layoutplan, Entwürfe je Version
└── fertig/                 # GETEILT: fertige Lieferungen
```

Regeln: Geteilte Dateien nie doppelt anlegen, nie paketspezifisch umbenennen. Änderungen an geteilten Dateien konservativ (ergänzen statt umbauen — das andere Paket liest mit). Jedes Paket schreibt Learnings NUR in seine eigene Learnings-Datei.

## Zwei Wissensebenen

- **Plugin = Allgemeinwissen** (Gestaltungsregeln, Formate, Technik): zentral gepflegt, kommt per Update. Nie lokal editieren/kopieren.
- **Kundenordner = Kundenwissen**: lebt lokal, überlebt jedes Update, wird bei jeder Aktivierung gelesen.

## Der Abschluss-Ritus (Pflicht nach jeder finalen Abnahme)

1. Alle Korrekturen der Feedbackrunden durchgehen (auch kleine: „Pill 0,5 mm tiefer", „Logo größer").
2. Kategorisieren — **gilt das nur für DIESEN Kunden oder für JEDEN?**
   - Kundenspezifisch (Geschmack, CI-Details, Layout-Präferenzen) → `design-learnings.md` mit Datum + Ein-Satz-Kontext; betrifft es Design-System-Werte → direkt `design-system.md` nachziehen.
   - Allgemeingültig (Fehler, den das System bei jedem machen würde) → NICHT in Kundendateien. Im Abschluss-Satz benennen („Beobachtung fürs Grundsystem: …") — die zentrale Pflege sammelt das im direkten Kontakt ein.
3. Dem Kunden in einem Satz sagen, was das System für ihn gelernt hat.

## Format der Einträge

```
## 2026-08-24 — Logo-Mindestgröße
**Kontext:** Feedback Flyer v3
**Learning:** Partnerlogos nie unter 12 mm Breite — Kunde empfindet kleiner als „versteckt".
**Umsetzung:** design-system.md, Abschnitt Logo-Regeln ergänzt.
```

Ein Learning ohne Umsetzungsnotiz ist nur eine Beobachtung.
