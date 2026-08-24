# Setup — einmalig pro Kunde

Ziel: Danach entsteht jedes Design ohne Rückfragen im richtigen Look und Wording.

## Schritt 0 — Geteilten Bestand prüfen (Interop mit AI Video-Cutter)

Zuerst im Arbeitsordner des Kunden nachsehen: Existieren `kunden-config.yaml` und `marken-profil.md` bereits (z. B. vom AI Video-Cutter angelegt)? Dann **übernehmen, nicht neu erheben** — CI, Kanäle und Brand Voice sind schon angelernt. Nur die design-spezifischen Teile ergänzen (Schritte 2–3). Fehlt beides: komplettes Setup fahren (Schritt 1–3).

## Schritt 1 — Basis-Setup (nur wenn kein geteilter Bestand existiert)

- **CI erfassen:** Website + Profile ansehen, Farben vorschlagen statt abfragen; genau EINE Akzentfarbe; Fonts (Familie + Schnitte) einsammeln; Logo-Originaldateien in `assets/`.
- **Kanäle:** Welche Plattformen bespielt der Kunde wirklich? Welche Formate braucht er regelmäßig (Posts, Carousels, Stories, Ads, Print)?
- **Marken-Analyse:** Website + alle Social-Accounts (mehrere pro Kanal erlaubt) analysieren → `marken-profil.md` mit belegten Mustern (Tonalität, Wortschatz, No-Go-Wörter, Hook-/CTA-Muster je Account). Kurzfassung dem Kunden zur Bestätigung zeigen.
- Ergebnis in `kunden-config.yaml` (Template beim AI Video-Cutter identisch — dieselbe Datei).

## Schritt 2 — Design-System erfassen (immer, das Herzstück dieses Pakets)

- **Referenz-Vorlagen einsammeln:** bestehende Flyer, Posts, Ads, Präsentationen des Kunden — die 3–7 besten in `vorlagen/` ablegen (PDF/PNG/Quelldateien). „Die Wahrheit" für jedes künftige Design.
- **Vorlagen auslesen** (nicht schätzen — Rezepte in `technik.md`): Schriften + Schnitte aus PDFs, Farben pipettieren, Maße und Abstände messen.
- **`design-system.md` schreiben:** Layout-Master je Format, Farbrollen (was ist Fläche, was Akzent, was Text), Typo-Skala (welcher Schnitt in welcher Größe wofür), wiederkehrende Elemente (Balken, Pills, Fußzeilen, QR-Positionen), Logo-Regeln, Foto-Stil. Jede Angabe mit Quelle (aus welcher Vorlage gelesen).
- Dem Kunden die Kernpunkte zeigen: „So baut eure Marke Designs — stimmt das?"

## Schritt 3 — Ordner & Abschluss

- Struktur ergänzen: `vorlagen/`, `projekte/`, `fertig/`, `design-learnings.md` (leer mit Kopfzeile). `assets/` wird mit dem Video-Cutter geteilt.
- **Probelauf anbieten:** ein echtes kleines Design (1 Post oder 1 Flyer-Nachbau) — kalibriert besser als jede Beschreibung.
