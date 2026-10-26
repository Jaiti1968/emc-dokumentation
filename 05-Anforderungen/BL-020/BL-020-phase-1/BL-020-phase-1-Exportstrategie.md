# BL-020 Phase 1 – Exportstrategie

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 1 – Reporting-Grundgerüst

---

# 1. Zielsetzung

Dieses Dokument definiert die Exportstrategie für das zukünftige Reporting der EMC Mitgliederverwaltung.

Ziel ist die Festlegung einer einheitlichen und wartbaren Lösung für die Ausgabe von Berichtsdaten.

Die Exportstrategie soll:

* mit der Reporting-Zielarchitektur kompatibel sein
* bestehende und zukünftige Berichte unterstützen
* doppelte Fachlogik vermeiden
* für einen Einzelentwickler dauerhaft wartbar bleiben

---

# 2. Grundsätze

## Vorschau vor Export

Jeder Bericht soll zunächst als Vorschau innerhalb der Anwendung angezeigt werden.

Exportfunktionen sind grundsätzlich nachgelagert.

Prinzip:

```text
Filter setzen
        ↓
Vorschau erzeugen
        ↓
optional exportieren
```

Der Export ersetzt nicht die Vorschau.

---

## Eine Datenbasis für alle Ausgaben

Die fachliche Datenermittlung erfolgt genau einmal.

```text
Report Service
        ↓
ReportResultDto
```

Auf dieser Datenbasis können verschiedene Ausgaben erzeugt werden.

Beispiele:

```text
Tabellenansicht

PDF

Excel

CSV
```

---

## Keine Exportlogik im Frontend

Das Frontend startet lediglich den Export.

Die eigentliche Dateierzeugung erfolgt ausschließlich im Backend.

Dadurch bleiben:

* Fachlogik
* Formatierungslogik
* Exportlogik

zentral an einer Stelle.

---

## Keine externe Reportingplattform

Für die EMC Mitgliederverwaltung wird keine eigenständige Reportingplattform vorgesehen.

Beispiele:

```text
JasperReports

BIRT

Crystal Reports
```

werden bewusst nicht eingesetzt.

Begründung:

* zusätzliche Komplexität
* zusätzlicher Wartungsaufwand
* geringe Projektgröße
* Einzelentwicklerbetrieb

---

# 3. Unterstützte Ausgabeformen

## Interaktive Vorschau

Primäre Ausgabeform.

Darstellung:

```text
Tabellenansicht
```

innerhalb der Anwendung.

Die Vorschau bildet die Grundlage aller weiteren Exportfunktionen.

---

## PDF

Verwendungszweck:

```text
Druck

Archivierung

Weitergabe
```

Typische Berichte:

```text
Mitgliederverzeichnis

Geburtstagsliste

Zugehörigkeitsauswertung

Mitgliederdatenblatt
```

---

## Excel

Verwendungszweck:

```text
Weiterverarbeitung

Sortierung

Filterung

individuelle Auswertung
```

Typische Berichte:

```text
Mitgliederverzeichnis

Geburtstagsliste

Kontrollberichte
```

---

## CSV

Verwendungszweck:

```text
Datenexport

Import in Fremdsysteme

technische Weiterverarbeitung
```

CSV wird als einfachstes Exportformat bereitgestellt.

---

# 4. Nicht vorgesehene Formate

Für die erste Reportinggeneration nicht vorgesehen:

```text
Word

OpenDocument

HTML-Reports

XML-Exporte
```

Diese Formate können später ergänzt werden.

---

# 5. Exportarchitektur

## Zielbild

```text
Frontend
        ↓
Reporting API
        ↓
Report Service
        ↓
ReportResultDto
        ↓
Export Service
        ↓
Datei
```

---

## Verantwortung des Report Service

Der Report Service liefert:

```text
fachlich aufbereitete Daten
```

Beispiel:

```text
MemberReportResultDto
```

Der Service kennt keine PDF-, Excel- oder CSV-Details.

---

## Verantwortung des Export Service

Der Export Service erzeugt die gewünschte Datei.

Beispiele:

```text
PdfExportService

ExcelExportService

CsvExportService
```

Der Export Service enthält keine fachliche Berechnungslogik.

---

# 6. PDF-Strategie

## Ziel

Erzeugung druckbarer Berichte.

---

## Grundsatz

PDFs basieren auf den Ergebnisdaten der jeweiligen Report Services.

```text
ReportResultDto
        ↓
PDF
```

---

## Layoutstrategie

Für die erste Reportinggeneration wird eine einfache Layoutstrategie empfohlen.

Merkmale:

```text
Vereinslogo optional

Titel

Erstellungszeitpunkt

Filterübersicht

Tabellarische Daten

Seitenzahlen
```

---

## Keine Pixelperfektion

Nicht Ziel:

```text
komplexe Drucksatzsysteme

exakte Access-Nachbildung

Layoutdesigner
```

Ziel sind funktionale und gut lesbare PDF-Berichte.

---

# 7. Excel-Strategie

## Ziel

Weiterverarbeitung durch Anwender.

---

## Grundsatz

Excel dient nicht primär dem Druck.

Excel dient:

```text
Sortieren

Filtern

Bearbeiten

Analysieren
```

---

## Aufbau

Empfohlen:

```text
Kopfbereich

Filterinformationen

Tabellendaten
```

---

## Formatierung

Nur grundlegende Formatierungen.

Beispiele:

```text
Überschriften

Datumsformate

Spaltenbreiten
```

Keine komplexen Excel-Makros.

---

# 8. CSV-Strategie

## Ziel

Einfacher Rohdatenexport.

---

## Eigenschaften

CSV enthält:

```text
Spaltenüberschriften

Datensätze
```

Keine Formatierungen.

Keine Layoutinformationen.

---

## Einsatzgebiet

Geeignet für:

```text
Importe

Weiterverarbeitung

Datenaustausch
```

---

# 9. Berichtsarten und Exportfähigkeit

## Mitgliederverzeichnis

Vorgesehen:

```text
Vorschau

PDF

Excel

CSV
```

---

## Mitgliederdatenblatt

Vorgesehen:

```text
Vorschau

PDF
```

Excel und CSV sind für diesen Bericht zunächst nicht erforderlich.

---

## Geburtstagsliste

Vorgesehen:

```text
Vorschau

PDF

Excel

CSV
```

---

## Zugehörigkeitsauswertung

Vorgesehen:

```text
Vorschau

PDF

Excel

CSV
```

---

## Kontrollberichte

Vorgesehen:

```text
Vorschau

Excel

CSV
```

PDF optional.

---

## Statistik

Vorgesehen:

```text
Vorschau

PDF
```

Weitere Formate werden später bewertet.

---

# 10. Dateibenennung

Empfohlene Struktur:

```text
reportname_yyyymmdd_hhmmss.ext
```

Beispiele:

```text
mitgliederverzeichnis_20260715_101500.pdf

geburtstagsliste_20260715_101500.xlsx
```

---

# 11. Berechtigungen

Exportfunktionen unterliegen denselben oder strengeren Berechtigungen wie die reine Anzeige.

Grundsatz:

```text
Anzeigen
≠
Exportieren
```

Die konkrete Berechtigungsmatrix wird im separaten Berechtigungskonzept definiert.

---

## Phase-1-Empfehlung

Für die erste Reportinggeneration wird ein konservativer Ansatz empfohlen.

```text
Reporting
→ ausschließlich ADMIN
```

Dadurch sind automatisch auch Exportfunktionen zunächst auf Administratoren beschränkt.

---

# 12. Fehlerbehandlung

Typische Fehlerfälle:

```text
Export nicht unterstützt

keine Daten vorhanden

ungültige Filter

Dateierzeugung fehlgeschlagen
```

Fehler werden über die bestehende zentrale Fehlerbehandlung verarbeitet.

---

# 13. Teststrategie

Empfohlene Tests:

```text
PDF-Erzeugung

Excel-Erzeugung

CSV-Erzeugung

Dateinamen

Fehlerfälle
```

Die Tests sollen prüfen:

```text
Datei erzeugt

Datei nicht leer

korrekter Dateityp

korrekte Metadaten
```

Nicht Ziel sind Layouttests auf Pixelebene.

---

# 14. Abgrenzung dieser Phase

Nicht Bestandteil von BL-020 Phase 1:

```text
konkrete PDF-Layouts

konkrete Excel-Layouts

konkrete CSV-Strukturen

konkrete Berichtsimplementierungen
```

Diese Themen werden in den jeweiligen Reportingphasen behandelt.

---

# 15. Zusammenfassung

Die Exportstrategie basiert auf vier Grundprinzipien:

```text
Vorschau vor Export

eine gemeinsame Datenbasis

Backend erzeugt Dateien

keine externe Reportingplattform
```

Unterstützte Formate:

```text
PDF

Excel

CSV
```

Die Exportarchitektur bleibt einfach, wartbar und vollständig kompatibel mit der Reporting-Zielarchitektur der EMC Mitgliederverwaltung.

Für die erste Reportinggeneration werden Reporting und Exportfunktionen ausschließlich Administratoren bereitgestellt.

