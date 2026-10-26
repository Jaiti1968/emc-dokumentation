# BL-020 Phase 2 – Mitgliederverzeichnis – Exportkonzept

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 2 – Mitgliederverzeichnis

---

# 1. Zielsetzung

Dieses Dokument beschreibt die Exportstrategie für den Report:

```text
Mitgliederverzeichnis
```

Ziel ist die Definition der Exportformate, Exportinhalte und Datenschutzanforderungen für den ersten produktiven Reporting-Baustein.

Das Konzept konkretisiert die in BL-020 Phase 1 festgelegte gemeinsame Exportstrategie.

---

# 2. Grundprinzipien

## Einheitliche Datenbasis

Alle Exportformate basieren auf derselben fachlichen Berichtsstruktur.

Architektur:

```text
MembersDirectoryReportService
            ↓
MembersDirectoryReportResponse
            ↓
PDF
Excel
CSV
```

Die fachliche Logik wird ausschließlich einmal implementiert.

---

## Vorschau und Export identisch

Die Exportdaten müssen fachlich identisch zur Browser-Vorschau sein.

Unterschiede dürfen ausschließlich entstehen durch:

```text
Layout
Dateiformat
Darstellung
```

Nicht jedoch durch unterschiedliche Fachlogik.

---

## Export als sekundäre Funktion

Das Mitgliederverzeichnis soll primär als digitale Auswertung genutzt werden.

Exporte dienen als:

- Druckausgabe
- Weiterverarbeitung
- Archivierung
- Weitergabe innerhalb des Vereins

---

# 3. Unterstützte Exportformate

## PDF

Status:

```text
Phase 2
Umsetzung empfohlen
```

Priorität:

```text
Sehr hoch
```

---

## Excel

Status:

```text
Phase 2
Umsetzung empfohlen
```

Priorität:

```text
Hoch
```

---

## CSV

Status:

```text
Architektonisch vorbereiten
Umsetzung optional
```

Priorität:

```text
Mittel
```

---

# 4. PDF-Konzept

## Zielsetzung

PDF dient als Nachfolger der bisherigen Access-Ausdrucke.

Anwendungsfälle:

- Vorstandsarbeit
- Probenbetrieb
- Sitzungsunterlagen
- Archivierung
- Ausdruck

---

## Unterstützte Layoutvarianten

### Standard

Darstellung:

```text
2-zeilig
```

Automatischer Seitenwechsel bei Gruppierungswechsel.

---

### Kompakt

Darstellung:

```text
1-zeilig
```

Automatischer Seitenwechsel bei Gruppierungswechsel.

---

### Fortlaufend

Darstellung:

```text
1-zeilig
```

Kein automatischer Seitenwechsel.

---

### Kammerchor

Verwendet dieselben Layoutvarianten.

Gruppierung ausschließlich nach Stimme.

---

# 5. PDF-Inhalte

## Grundsatz

PDF verwendet dieselbe Datenbasis wie die Browser-Vorschau.

---

## Verfügbare Daten

```text
Mitgliedsnummer
Nachname
Vorname
Straße
PLZ
Ort
Geburtsdatum
Telefon privat
Telefon geschäftlich
Mobiltelefon
E-Mail
Mitgliedsstatus
Stimme
Kammerchor
```

---

## Layoutabhängige Darstellung

Die tatsächlich sichtbaren Felder können je nach Layoutvariante unterschiedlich angeordnet werden.

Die fachliche Datenbasis bleibt unverändert.

---

# 6. PDF-Dokumentstruktur

## Kopfbereich

Enthält:

```text
EMC Mitgliederverwaltung

Mitgliederverzeichnis

Erstellungsdatum

gewählte Variante

aktive Filter
```

---

## Gruppierungsbereich

Anzeige der jeweiligen Gruppenüberschriften.

Beispiele:

```text
Aktive Mitglieder

1. Tenor
```

oder:

```text
Passive Mitglieder
```

---

## Datenbereich

Anzeige der eigentlichen Mitgliederdaten.

---

## Fußbereich

Optional:

```text
Seitenzahl
```

---

# 7. Excel-Konzept

## Zielsetzung

Excel dient der Weiterverarbeitung von Mitgliederdaten.

Anwendungsfälle:

- Sonderauswertungen
- Sortierungen
- Filterungen
- Serienbriefvorbereitung
- Vereinsinterne Datenverarbeitung

---

## Struktur

Ein Arbeitsblatt:

```text
Mitgliederverzeichnis
```

---

## Aufbau

Jede Zeile repräsentiert genau ein Mitglied.

Beispiel:

```text
Mitgliedsnummer
Nachname
Vorname
Ort
E-Mail
...
```

---

## Gruppierungen

Gruppierungen werden in Excel nicht als Seitenstruktur abgebildet.

Die Daten werden als flache Tabelle exportiert.

Die fachliche Gruppierungslogik bleibt jedoch in der Sortierung erhalten.

---

## Sortierung

Die fachliche Sortierung bleibt auch im Excel-Export erhalten.

Sortierreihenfolge:

```text
Mitgliedsstatus
↓
Stimme
↓
Nachname
↓
Vorname
```

Dabei gilt:

- aktive Mitglieder werden zusätzlich nach Stimme gruppiert
- bei anderen Mitgliedsstatus ist die Stimme fachlich nicht relevant
- innerhalb jeder Gruppe erfolgt die Sortierung nach Nachname und Vorname

Dadurch entspricht die Reihenfolge der Datensätze der Browser-Vorschau und den PDF-Ausgaben.

---

## Vorteile

Durch die Beibehaltung der fachlichen Sortierung:

- bleibt die Wiedererkennbarkeit der Mitgliederlisten erhalten
- entsprechen Excel-Exporte dem bisherigen Access-Verhalten
- können Auswertungen ohne zusätzliche Sortierung genutzt werden
- bleiben Browser-Vorschau, PDF und Excel fachlich konsistent

---

# 8. CSV-Konzept

## Zielsetzung

CSV dient dem Datenaustausch.

Anwendungsfälle:

- Fremdsysteme
- individuelle Auswertungen
- Datenimporte

---

## Struktur

Flache Datentabelle.

Keine Gruppierungen.

Keine Layoutinformationen.

Keine Formatierungen.

---

## Aufbau

Jede Zeile repräsentiert genau ein Mitglied.

Die erste Zeile enthält die Spaltenüberschriften.

Beispiel:

```text
Mitgliedsnummer;Nachname;Vorname;Ort;E-Mail;...
```

---

## Sortierung

Die fachliche Sortierung bleibt auch im CSV-Export erhalten.

Sortierreihenfolge:

```text
Mitgliedsstatus
↓
Stimme
↓
Nachname
↓
Vorname
```

Dabei gilt:

- aktive Mitglieder werden zusätzlich nach Stimme gruppiert
- bei anderen Mitgliedsstatus ist die Stimme fachlich nicht relevant
- innerhalb jeder Gruppe erfolgt die Sortierung nach Nachname und Vorname

Dadurch entspricht die Reihenfolge der Datensätze der Browser-Vorschau, den PDF-Ausgaben und den Excel-Exporten.

---

## Zeichensatz

Empfohlen:

```text
UTF-8
```

Dadurch werden Umlaute und Sonderzeichen korrekt übertragen.

---

## Umsetzung

CSV wird architektonisch vorbereitet.

Eine Umsetzung in Phase 2 ist nicht zwingend erforderlich.

---

## Vorteile

Durch die Beibehaltung der fachlichen Sortierung:

- bleibt die Konsistenz zu allen anderen Ausgabeformaten erhalten
- entspricht der Export dem bisherigen Access-Verhalten
- wird eine nachträgliche Sortierung häufig überflüssig
- können CSV-Dateien direkt weiterverarbeitet werden

---

# 9. Dateinamenskonventionen

## Grundsatz

Dateinamen sollen eindeutig und nachvollziehbar sein.

---

## PDF

Beispiel:

```text
mitgliederverzeichnis-standard-2026-06-20.pdf
```

---

## Excel

Beispiel:

```text
mitgliederverzeichnis-standard-2026-06-20.xlsx
```

---

## CSV

Beispiel:

```text
mitgliederverzeichnis-standard-2026-06-20.csv
```

---

# 10. Datenschutzbewertung

## Personenbezogene Daten

Das Mitgliederverzeichnis enthält personenbezogene Daten.

Insbesondere:

```text
Name
Adresse
Telefonnummern
E-Mail-Adressen
Geburtsdatum
Mitgliedschaftsdaten
```

---

## Risiko durch Exporte

Im Gegensatz zur Browser-Vorschau entstehen bei Exporten eigenständige Dateien.

Diese können:

- gespeichert werden
- weitergegeben werden
- ausgedruckt werden

Dadurch entsteht ein höheres Datenschutzrisiko.

---

## Grundsatz

Exportfunktionen dürfen ausschließlich berechtigten Benutzern zur Verfügung stehen.

Phase 2:

```text
Nur ADMIN
```

---

# 11. Datenschutzempfehlungen

## PDF

Geeignet für:

```text
Vorstand
Organisation
Probenbetrieb
```

---

## Excel

Geeignet für:

```text
interne Weiterverarbeitung
```

Besondere Sorgfalt erforderlich.

---

## CSV

Nur bei tatsächlichem Bedarf verwenden.

CSV besitzt keine Formatierung und keine zusätzlichen Schutzmechanismen.

---

# 12. Technische Umsetzungsempfehlung

## PDF

Empfohlen:

```text
MembersDirectoryPdfExporter
```

---

## Excel

Empfohlen:

```text
MembersDirectoryExcelExporter
```

---

## CSV

Empfohlen:

```text
MembersDirectoryCsvExporter
```

---

## Gemeinsame Basis

Alle Exporter erhalten dieselbe Eingabestruktur:

```text
MembersDirectoryReportResponse
```

Dadurch bleibt die Fachlogik zentralisiert.

---

# 13. Teststrategie

## PDF

Prüfen:

- Dokument erzeugbar
- Gruppierungen korrekt
- Seitenwechsel korrekt
- Layoutvarianten korrekt

---

## Excel

Prüfen:

- Datei erzeugbar
- Spalten vollständig
- Sortierung korrekt

---

## CSV

Prüfen:

- Trennzeichen korrekt
- Zeichensatz korrekt
- Datensätze vollständig

---

# 14. Entscheidungsempfehlung

Für Phase 2 wird empfohlen:

```text
PDF
=
Umsetzen

Excel
=
Umsetzen

CSV
=
Vorbereiten
```

Begründung:

PDF und Excel decken die wesentlichen Anwendungsfälle der bisherigen Access-Lösung ab.

CSV besitzt aktuell keinen erkennbaren fachlichen Mehrwert und kann später ergänzt werden.

---

# 15. Abschlussbewertung

Das Mitgliederverzeichnis eignet sich sehr gut für eine gemeinsame Exportarchitektur.

Die vorgeschlagene Lösung:

```text
Reporting-Service
↓
gemeinsame Berichtsstruktur
↓
PDF / Excel / CSV
```

entspricht der in BL-012 und BL-020 definierten Reportingstrategie.

Dadurch entsteht:

- eine einheitliche Architektur
- geringe Wartungskosten
- hohe Wiederverwendbarkeit
- einfache Erweiterbarkeit

für zukünftige Reporting-Funktionen der EMC Mitgliederverwaltung.
