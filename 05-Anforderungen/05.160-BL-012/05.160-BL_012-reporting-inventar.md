# 05.160-BL_012-reporting-inventar.md

Status: Discovery abgeschlossen

Projekt: EMC Mitgliederverwaltung

Anforderung: BL-012 Reportingstrategie

Vorgängerphase:

- BL-012a Reporting Discovery

Zweck:

Dieses Dokument beschreibt die bestehende Reporting-, Export- und Dokumentgenerierungslandschaft der Access-basierten EMC-Mitgliederverwaltung.

Es dient als fachliche und technische Grundlage für die spätere Reportingstrategie (BL-012b) sowie für die schrittweise Ablösung der Access-Anwendung.

---

# 1. Zusammenfassung

Die Discovery-Phase BL-012a hat gezeigt, dass die Reporting-Landschaft der bestehenden Access-Anwendung deutlich überschaubarer ist als ursprünglich angenommen.

Die meisten Berichte dienen:

- der Mitgliederverwaltung
- der Ehrungsvorbereitung
- der Ehrungshistorie
- der Vereinsorganisation
- der Datenkontrolle
- einfachen statistischen Auswertungen

Die vorhandenen Berichte werden überwiegend:

- als Druckvorschau betrachtet
- als PDF erzeugt
- auf Papier ausgegeben

Interaktive Dashboards oder Analysefunktionen spielen aktuell keine wesentliche Rolle.

Die eigentliche Komplexität liegt überwiegend in der Fachlogik und den historisch gewachsenen Querystrukturen, nicht in der Berichtserzeugung selbst.

---

# 2. Berichtskategorien

## 2.1 Operative Vereinsberichte

Geschäftskritische Berichte zur Vorbereitung und Durchführung von Vereinsprozessen.

### rptGeburtstagsliste

Zweck:

Vorbereitung der Geburtstagsgratulation während der Chorprobe.

Nutzer:

- Präsident

Fachliche Bedeutung:

- Anzeige der Geburtstage
- Anzeige des aktuellen Alters
- Kennzeichnung von Jubiläumsgeburtstagen

Jubiläumsgeburtstage:

- 60
- 65
- 70
- 75
- 80
- 85
- 90
- 95
- 100

Besonderheiten:

- Jubiläumsgeburtstage werden hervorgehoben
- Weinpräsent bei Jubiläumsgeburtstagen

Wichtige fachliche Erkenntnis:

Geburtstagsgratulationen sind keine Ehrungen.

Sie werden nicht in der Ehrungshistorie gespeichert.

Datenquelle:

- qryGeburtstagsliste

Komplexität:

Mittel

Migrationsfähigkeit:

Sehr hoch

---

### rptZugehoerigkeit

Zweck:

Ermittlung von Mitgliedern, die aufgrund ihrer Vereinszugehörigkeit für Ehrungen in Frage kommen.

Nutzer:

- Vorstand

Vereinsprozess:

Vorbereitung der Ehrungen zur Jahreshauptversammlung.

Datenquelle:

Querykette Ehrungswesen

Komplexität:

Mittel bis hoch

Migrationsfähigkeit:

Hoch

---

### rptZugehoerigkeitnachJahrensortiert

Zweck:

Alternative Darstellung der Ehrungsvorschläge nach Zugehörigkeitsjahren.

Nutzer:

- Vorstand

Vereinsprozess:

Vorbereitung der Ehrungen zur Jahreshauptversammlung.

Datenquelle:

Querykette Ehrungswesen

Komplexität:

Mittel bis hoch

Migrationsfähigkeit:

Hoch

---

### rptEhrungenvonMitgliedern

Zweck:

Darstellung der Historie bereits vergebener Ehrungen.

Nutzer:

- Vorstand

Datenbasis:

- tblEhrungen
- tblEhrungen_FT

Komplexität:

Mittel

Migrationsfähigkeit:

Hoch

---

### rptEhrungenvonEhrenmitgliedern

Zweck:

Spezialauswertung für Ehrenmitglieder.

Fachlich:

Variante von rptEhrungenvonMitgliedern.

Komplexität:

Niedrig

Migrationsfähigkeit:

Hoch

---

# 2.2 Mitgliederverzeichnis

## Fachliche Betrachtung

Die Analyse hat ergeben, dass mehrere Access-Berichte fachlich dieselbe Auswertung darstellen.

Die Unterschiede liegen ausschließlich in:

- Layout
- Filterung
- Seitenumbrüchen

Langfristig sollten diese Berichte als Varianten eines gemeinsamen Berichtstyps betrachtet werden.

---

### rptMitgliederliste

Merkmale:

- zweizeilige Darstellung
- Gruppierung nach Stimme
- Gruppierung nach Mitgliederstatus
- Seitenumbruch bei Gruppenwechsel

Nutzung:

- Vorstand
- Chorleiter
- Stimmführer

Datenquelle:

- qryMitgliederliste

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

### rptMitgliederlistekompakt

Merkmale:

- einzeilige Darstellung
- Seitenumbruch je Gruppe

Datenquelle:

- qryMitgliederliste

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

### rptMitgliederlistefortlaufend

Merkmale:

- einzeilige Darstellung
- keine Seitenumbrüche bei Gruppenwechsel

Datenquelle:

- qryMitgliederliste

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

### rptMitgliederlisteKammerchor

Merkmale:

- Darstellung wie kompakte Mitgliederliste
- Filterung auf Kammerchor-Mitglieder

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

# 2.3 Mitgliederbezogene Dokumente

### rptMitgliederdatenblatt

Zweck:

Ausgabe der aktuell gespeicherten Mitgliedsdaten zur Überprüfung durch das Mitglied.

Anwendung:

- Ausdruck
- Verteilung an Mitglieder
- handschriftliche Korrekturen
- Rückgabe an Administrator

Besonderheit:

Enthält den Subreport:

```text
rptEhrungen
```

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

# 2.4 Kontroll- und Übersichtsberichte

### rptDatenschutz

Zweck:

Kontrolle der Datenschutzinformationen.

Nutzung:

Bei Bedarf.

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

### rptChorkleidung

Zweck:

Kontrolle der ausgegebenen Chorkleidung.

Besonderheit:

Mehrere Datensätze pro Mitglied möglich.

Nutzung:

Bei Bedarf.

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

### rptVerteiler

Zweck:

Kontrolle der Zuordnung von E-Mail-Adressen zu Verteilern.

Datenbasis:

- tblVerteiler
- tblVerteiler_FT

Nutzung:

Bei Bedarf.

Komplexität:

Niedrig

Migrationsfähigkeit:

Sehr hoch

---

# 3. Fachlogik-Katalog

## 3.1 Geburtstagswesen

Zweck:

Vorbereitung der Geburtstagsgratulation während der Chorproben.

Wichtige fachliche Regeln:

### Jubiläumsgeburtstage

- 60 Jahre
- 65 Jahre
- 70 Jahre
- 75 Jahre
- 80 Jahre
- 85 Jahre
- 90 Jahre
- 95 Jahre
- 100 Jahre

Besonderheiten:

- Hervorhebung im Bericht
- Weinpräsent bei Jubiläumsgeburtstagen

Wichtige fachliche Trennung:

Geburtstagsgratulationen sind keine Ehrungen.

Es erfolgt keine Speicherung in:

```text
tblEhrungen
```

---

## 3.2 Ehrungswesen

### EMC-Ehrungen

Grundlage:

Dauer der Mitgliedschaft im EMC.

Verwendung:

- Ehrungsvorschläge
- Jahreshauptversammlung
- Ehrungshistorie

---

### DCV-Ehrungen

Grundlage:

Dauer der Mitgliedschaft im Deutschen Chorverband.

Besonderheit:

Kann von der EMC-Mitgliedschaft abweichen.

Verwendung:

- Ehrungsvorschläge
- Jahreshauptversammlung
- Ehrungshistorie

---

### Besondere Ehrungen

Vorhanden:

- Ehrenmitglied
- Ehrenpräsident

---

## 3.3 Ehrungsmodell

### tblEhrungen_FT

Fachtabelle aller möglichen Ehrungsarten.

Beispiele:

- 20 Jahre EMC
- 25 Jahre EMC
- 30 Jahre EMC
- 40 Jahre EMC
- 50 Jahre EMC
- 20 Jahre DCV
- 25 Jahre DCV
- 30 Jahre DCV
- 40 Jahre DCV
- 50 Jahre DCV
- Ehrenmitglied
- Ehrenpräsident

---

### tblEhrungen

Historie tatsächlich durchgeführter Ehrungen.

Beziehung:

```text
tblEhrungen
→ tblEhrungen_FT
```

---

# 4. Query-Landkarte

## 4.1 Mitgliederverzeichnis

### rptMitgliederliste

```text
rptMitgliederliste
└── qryMitgliederliste
    ├── tblMitglieder
    ├── tblKontaktdaten
    ├── tblMitgliedschaft
    ├── tblMitgliederstatus_FT
    └── tblStimme_FT
```

---

### rptMitgliederlistekompakt

```text
rptMitgliederlistekompakt
└── qryMitgliederliste
```

---

### rptMitgliederlistefortlaufend

```text
rptMitgliederlistefortlaufend
└── qryMitgliederliste
```

---

### rptMitgliederlisteKammerchor

```text
rptMitgliederlisteKammerchor
└── Variante von qryMitgliederliste
```

---

## 4.2 Geburtstagswesen

### rptGeburtstagsliste

```text
rptGeburtstagsliste
└── qryGeburtstagsliste
    ├── tblMitglieder
    ├── tblMitgliedschaft
    ├── tblMitgliederstatus_FT
    ├── tblStimme_FT
    └── tblAllgemein_FT
```

Berechnungen:

- Alter
- Altersgruppen
- Jubiläumskennzeichen

---

## 4.3 Ehrungswesen

### rptZugehoerigkeit

```text
rptZugehoerigkeit
└── Querykette Ehrungswesen
```

---

### rptZugehoerigkeitnachJahrensortiert

```text
rptZugehoerigkeitnachJahrensortiert
└── Querykette Ehrungswesen
```

---

### rptEhrungenvonMitgliedern

```text
rptEhrungenvonMitgliedern
└── tblEhrungen
    └── tblEhrungen_FT
```

---

### rptEhrungenvonEhrenmitgliedern

```text
rptEhrungenvonEhrenmitgliedern
└── Variante von rptEhrungenvonMitgliedern
```

---

## 4.4 Kontrollberichte

### rptVerteiler

```text
rptVerteiler
└── tblVerteiler
    └── tblVerteiler_FT
```

---

### rptDatenschutz

```text
rptDatenschutz
└── Datenschutzdaten
```

---

### rptChorkleidung

```text
rptChorkleidung
└── Chorkleidungsdaten
```

---

## 4.5 Statistik

### rptAltersdurchschnitt

```text
rptAltersdurchschnitt
└── Altersstatistik-Queries
```

---

### rptAltersstruktur

```text
Querykette A
↓
qryAltersstruktur_Chor

↓
Makro

↓
VBA-Modul Tabellen_Transponieren

↓
tblAltersstruktur_Chor_Transponiert

↓
Querykette B

↓
rptAltersstruktur
    └── rptAltersdurchschnitt
```

---

# 5. Exportfunktionen

## 5.1 Excel-Export Mitgliederliste

Makro:

```text
EMCMitgliederliste_ExportExcel
```

Datenquelle:

```text
qryMitgliederliste_ExportExcel
```

Ausgabe:

```text
Mitgliederliste_ExportExcel.xlsx
```

Nutzung:

- Abfragelisten
- externe Auswertungen
- Datenquelle für Serienbriefe außerhalb Access

Bewertung:

Niedrige Komplexität.

---

## 5.2 Weitere Exporte

Vorhanden:

- Excel
- CSV

Umsetzung:

Nutzung der Standardexportfunktionen von Access.

Die eigentliche Logik befindet sich überwiegend in den zugrunde liegenden Queries.

---

# 6. Dokumentgenerierung

## 6.1 Nutzungsvereinbarung Chorkleidung

Auslöser:

Button:

```text
Nutzungsvereinbarung erstellen
```

Bereich:

```text
Formular Chorkleidung
```

---

## 6.2 Technische Umsetzung

Technologie:

- Access VBA
- Word Automation
- Bookmarks

Vorlage:

```text
Nutzungsvereinbarung_Chorkleidung.dotm
```

Verarbeitung:

- aktueller Datensatz
- keine Massenverarbeitung
- keine Serienbriefverarbeitung

---

## 6.3 Technische Bewertung

Aktuelle Einschränkung:

Fest verdrahteter lokaler Dateipfad.

Bewertung:

Single-User-Lösung.

Empfehlung:

Zentrale Vorlagenablage auf NAS.

---

# 7. VBA- und Makroinventar

## 7.1 Tabellen_Transponieren

Zweck:

Transponierung von Query-Ergebnissen.

Verwendung:

Ausschließlich für rptAltersstruktur.

Aufgabe:

```text
Query
→ Zeilen und Spalten tauschen
→ Hilfstabelle erzeugen
```

---

## 7.2 transponierenAltersstruktur_Chor

Zweck:

Ausführung der Transponierung für die Altersstruktur.

Verwendung:

```text
qryAltersstruktur_Chor
→ tblAltersstruktur_Chor_Transponiert
```

---

## Bewertung

Die VBA-Logik enthält keine fachliche Vereinslogik.

Sie dient ausschließlich der technischen Datenaufbereitung.

---

# 8. Komplexitätsmatrix

| Bereich               | Bewertung |
| --------------------- | --------- |
| Mitgliederverzeichnis | Niedrig   |
| Geburtstagsliste      | Mittel    |
| Ehrungsvorschläge     | Hoch      |
| Ehrungshistorie       | Mittel    |
| Datenschutz           | Niedrig   |
| Chorkleidung          | Niedrig   |
| Verteiler             | Niedrig   |
| Mitgliederdatenblatt  | Niedrig   |
| Altersdurchschnitt    | Niedrig   |
| Altersstruktur        | Mittel    |

---

# 9. Konsolidierungspotential

## Mitgliederlistenfamilie

Heute:

- rptMitgliederliste
- rptMitgliederlistekompakt
- rptMitgliederlistefortlaufend
- rptMitgliederlisteKammerchor

Zielbild:

```text
Mitgliederverzeichnis
```

mit Parametern:

- Standard
- Kompakt
- Fortlaufend
- Kammerchor

---

## Ehrungswesen

Zielbild:

```text
Ehrungsmodul
```

mit:

- Ehrungsvorschlägen
- Ehrungshistorie
- Ehrungsarten
- PDF-Ausgaben

---

## Kontrollberichte

Zielbild:

Filterbare Tabellenansichten mit:

- PDF-Export
- Excel-Export
- CSV-Export

---

# 10. Technische Altlasten

## Altersstruktur

Technische Umsetzung:

```text
Query
↓
Makro
↓
VBA
↓
Hilfstabelle
↓
weitere Query
↓
Diagramm
```

Zusätzlich:

Manueller Aktualisierungsschritt erforderlich.

Bewertung:

Technische Altlast.

Empfehlung:

Nicht nachbauen.

Fachliche Auswertung übernehmen.

Technische Umsetzung vollständig neu konzipieren.

---

## Word-Vorlage

Aktuell:

Lokaler fester Dateipfad.

Bewertung:

Technische Altlast.

Empfehlung:

Zentrale Vorlagenverwaltung.

---

# 11. Migrationsfähigkeit

## Sehr gut migrierbar

- Mitgliederverzeichnis
- Geburtstagsliste
- Ehrungshistorie
- Datenschutz
- Chorkleidung
- Verteiler
- Mitgliederdatenblatt
- Altersdurchschnitt
- Exportfunktionen

---

## Gut migrierbar

- Zugehörigkeit
- Zugehörigkeit nach Jahren sortiert

---

## Neu zu konzipieren

- Altersstruktur

---

# 12. Offene Punkte für BL-012b

Zu bewerten:

- Zielarchitektur
- PDF-Strategie
- Excel-/CSV-Strategie
- Dokumentgenerierung
- Rollenmodell
- Berechtigungen
- Konsolidierungspotential
- Dashboards
- EMC-Finanzen-Kompatibilität

---

# 13. Discovery-Fazit

Die Discovery hat gezeigt, dass die bestehende Reporting-Landschaft der EMC-Mitgliederverwaltung fachlich und technisch beherrschbar ist.

Die größte Herausforderung liegt nicht in der Berichtserzeugung selbst, sondern in:

- der Dokumentation der Fachlogik
- der Konsolidierung ähnlicher Berichte
- der Entflechtung historischer Querystrukturen

Eine vollständige Ablösung des Access-Reportings erscheint realistisch.

Empfohlen wird die Fortsetzung mit:

```text
BL-012b Reportingstrategie
```

zur Definition der zukünftigen Reporting- und Auswertungsarchitektur.
