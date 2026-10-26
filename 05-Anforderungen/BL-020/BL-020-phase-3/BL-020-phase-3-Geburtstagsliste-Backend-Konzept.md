# BL-020 Phase 3 – Geburtstagsliste

# Backend-Konzept

Stand: 03.07.2026

Status: Entwurf

Anforderung: BL-020 Reporting

Teilphase: Phase 3 – Geburtstagsliste

---

# Inhaltsverzeichnis

1. Zielsetzung
2. Ausgangssituation
3. Architekturprinzipien
4. Zielarchitektur
5. Backend-Komponenten
6. Datenmodell
7. Repository
8. Service-Schicht
9. Browserausgabe
10. PDF-Erzeugung
11. Wiederverwendbare Fachlogik
12. Fehlerbehandlung
13. Tests
14. Erweiterbarkeit

---

# 1. Zielsetzung

Die Geburtstagsliste erweitert die bestehende Reporting-Plattform um den zweiten Berichtstyp.

Dabei sollen insbesondere folgende Ziele erreicht werden:

- Wiederverwendung der bestehenden Reporting-Infrastruktur
- Trennung von Datenbeschaffung und Fachlogik
- Trennung von Fachlogik und Darstellung
- Einführung eines Hochformat-PDFs
- Erweiterbarkeit für zukünftige Berichte

---

# 2. Ausgangssituation

Mit Phase 2 wurden bereits eingeführt:

- ReportingController
- ReportingService
- ReportingRepository
- Browseransicht
- PDF-Export
- gemeinsame DTO-Struktur
- LandscapeReportTemplate
- MembersDirectoryPdfService

Diese Komponenten werden unverändert weiter genutzt.

Die Geburtstagsliste ergänzt die vorhandene Architektur um weitere Berichtskomponenten.

---

# 3. Architekturprinzipien

Für die Umsetzung gelten weiterhin die Grundsätze der Reportingstrategie.

## Wiederverwendung

Bestehende Komponenten werden bevorzugt erweitert.

Neue Komponenten entstehen nur bei echtem Wiederverwendungsbedarf.

---

## Trennung der Verantwortlichkeiten

Repository

- Daten lesen

Service

- Fachlogik

PDF

- Darstellung

Controller

- Bereitstellung der Reports

---

## Keine Fachlogik im Export

PDF- und Browserausgabe erhalten ausschließlich bereits vollständig aufbereitete Berichtsdaten.

Berechnungen erfolgen ausschließlich innerhalb der Service-Schicht.

---

# 4. Zielarchitektur

Die bestehende Struktur wird erweitert.

```text
reporting
│
├── controller
│
├── repository
│
├── service
│   ├── ReportingService
│   ├── BirthdayReportService
│   └── ...
│
├── export
│   ├── pdf
│   │
│   ├── PortraitReportTemplate
│   ├── BirthdayPdfService
│   └── ...
│
└── dto
```

Die vorhandene Architektur bleibt vollständig erhalten.

---

# 5. Backend-Komponenten

Für Phase 3 entstehen folgende neue Komponenten.

## BirthdayReportService

Verantwortlich für:

- Datenaufbereitung
- Altersberechnung
- Monatsgruppierung
- Sortierung
- Jubiläumskennzeichnung

---

## BirthdayPdfService

Verantwortlich für:

- Erstellung des PDF-Berichts
- Nutzung des PortraitReportTemplate

Keine fachlichen Berechnungen.

---

## PortraitReportTemplate

Neues gemeinsames Basistemplate.

Übernimmt:

- Logo
- Überschrift
- Geschäftsjahr
- Seitenkopf
- Seitenfuß
- Seitenzahlen
- Standdatum

Analog zum bestehenden LandscapeReportTemplate.

---

# 6. Datenmodell

Das Repository liefert ausschließlich Rohdaten.

Beispielsweise:

- Mitgliederstatus
- Stimme
- Vorname
- Nachname
- Geburtsdatum
- Geschäftsjahr

Zusätzliche fachliche Informationen werden anschließend im Service ergänzt.

Berechnete Felder:

- Monat
- Monatsname
- Geburtstag
- Alter
- Jubiläum

Dadurch bleiben Repository und SQL unabhängig von fachlichen Regeln.

---

# 7. Repository

Das ReportingRepository wird um eine weitere Methode ergänzt.

Beispielsweise:

```java
findBirthdayReport(...)
```

Die Methode liefert ausschließlich die benötigten Grunddaten.

Keine Berechnung von:

- Alter
- Jubiläum
- Gruppierungen

Diese verbleiben vollständig im Service.

---

# 8. Service-Schicht

Der BirthdayReportService übernimmt die vollständige fachliche Aufbereitung.

Aufgaben:

- Berechnung des Alters
- Kennzeichnung runder Geburtstage
- Ermittlung von Monat und Tag
- Sortierung
- Gruppierung
- Aufbau des Report-DTO

Die Service-Schicht bildet damit die zentrale fachliche Instanz des Berichts.

---

# 9. Browserausgabe

Die Browseransicht verwendet dieselbe Datenbasis wie das PDF.

Es existiert keine separate Browserlogik.

Der Controller liefert ausschließlich das Report-DTO.

Die Darstellung erfolgt im Frontend.

---

# 10. PDF-Erzeugung

Für die Geburtstagsliste wird ein eigener PDF-Service eingeführt.

Dieser verwendet:

```text
PortraitReportTemplate
```

Die PDF-Komponente übernimmt ausschließlich:

- Seitenlayout
- Tabellenaufbau
- Schriftarten
- Farben
- Zeilenumbrüche
- Seitenwechsel

Die rote Darstellung der Jubiläumsgeburtstage erfolgt ausschließlich auf Basis des bereits berechneten Jubiläumskennzeichens.

---

# 11. Wiederverwendbare Fachlogik

Die Geburtstagsliste führt erstmals allgemeine fachliche Berechnungen ein.

Diese sollen bewusst unabhängig vom eigentlichen Bericht implementiert werden.

## Jahresbezogene Differenzberechnung

Verwendung:

- Alter
- Vereinszugehörigkeit
- spätere Ehrungsberechnungen

Die Implementierung erfolgt als gemeinsame fachliche Hilfsmethode.

Beispiel:

```java
calculateYearsReachedInYear(...)
```

Die Methode beschreibt bewusst keine Geburtstage, sondern eine allgemeine jahresbezogene Differenzberechnung.

---

## Jubiläumsprüfung

Die Jubiläumsregel wird ebenfalls gekapselt.

Fachregel:

```text
Alter >= 60

UND

Alter mod 5 = 0
```

Dadurch wird die bisherige Access-Logik

```text
60
65
70
...
```

durch eine allgemein gültige Fachregel ersetzt.

---

# 12. Fehlerbehandlung

Mitglieder ohne Geburtsdatum werden nicht ausgeschlossen.

Stattdessen:

Geburtsdatum

```text
unbek.
```

Alter

leer

Jubiläum

false

Damit bleibt der Bericht vollständig und weist gleichzeitig auf unvollständige Stammdaten hin.

---

# 13. Tests

Zusätzlich zu den bestehenden Reporting-Tests werden neue Unit Tests vorgesehen.

Abzusichern sind insbesondere:

## Altersberechnung

- verschiedene Geburtsjahre
- unterschiedliche Geschäftsjahre

---

## Jubiläumsprüfung

Beispiele:

- 60
- 65
- 70
- kein Jubiläum

---

## Sortierung

Prüfung:

- Monat
- Tag
- Nachname
- Vorname

---

## Gruppierung

Prüfung der Monatswechsel.

---

## Fehlende Geburtsdaten

Prüfung der korrekten Darstellung.

---

## PDF

Erfolgreiche Erzeugung eines Hochformat-Berichts.

---

# 14. Erweiterbarkeit

Die in Phase 3 eingeführten Komponenten bilden die Grundlage weiterer Reports.

Insbesondere:

- Zugehörigkeitsliste
- Ehrungsvorschläge
- Altersstatistik
- weitere Hochformat-Berichte

Durch die konsequente Trennung von Repository, Service und Darstellung können zukünftige Berichte dieselben fachlichen Berechnungen wiederverwenden.

---

# Zusammenfassung

Mit der Geburtstagsliste wird die bestehende Reporting-Plattform erstmals um wiederverwendbare fachliche Berechnungen erweitert.

Die Umsetzung folgt konsequent den bereits eingeführten Architekturprinzipien:

- Datenbeschaffung im Repository
- Fachlogik im Service
- Darstellung im PDF-Template
- gemeinsame Datenbasis für Browser und PDF

Gleichzeitig entsteht mit dem PortraitReportTemplate die Grundlage für zukünftige Hochformat-Berichte und mit der allgemeinen jahresbezogenen Differenzberechnung ein erster wiederverwendbarer fachlicher Baustein für weitere Reportingfunktionen.