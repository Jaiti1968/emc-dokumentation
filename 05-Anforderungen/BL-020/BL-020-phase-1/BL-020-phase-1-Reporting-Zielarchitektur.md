# BL-020 Phase 1 – Reporting-Zielarchitektur

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 1 – Reporting-Grundgerüst

---

# 1. Zielsetzung

Dieses Dokument definiert die technische Zielarchitektur des zukünftigen Reporting-Subsystems der EMC Mitgliederverwaltung.

Ziel ist die Schaffung einer gemeinsamen Architektur für:

- Frontend
- Backend
- Reporting-Services
- Exportfunktionen

Die Architektur soll für alle zukünftigen Reportingphasen wiederverwendbar sein.

---

# 2. Architekturgrundsätze

## Kein Bericht als Einzellösung

Berichte werden künftig nicht als voneinander unabhängige Speziallösungen implementiert.

Stattdessen sollen gemeinsame Mechanismen genutzt werden.

Ziel:

```text
ein Bericht
=
eine Konfiguration
+
gemeinsame Infrastruktur
```

Nicht:

```text
ein Bericht
=
eigene Architektur
```

---

## Fachlogik nur einmal implementieren

Fachliche Regeln werden ausschließlich im Backend umgesetzt.

Beispiele:

- Geburtstagsberechnungen
- Altersberechnungen
- Zugehörigkeitsjahre
- Jubiläumslogik

Die Fachlogik darf nicht mehrfach für:

- PDF
- Excel
- CSV
- Frontend

implementiert werden.

---

## Trennung von Daten und Darstellung

Die Ermittlung der Daten erfolgt unabhängig von der späteren Ausgabe.

Dadurch kann dieselbe Auswertung mehrfach genutzt werden.

Beispiel:

```text
Geburtstagsauswertung
        ↓
Tabellenansicht

Geburtstagsauswertung
        ↓
PDF

Geburtstagsauswertung
        ↓
Excel
```

---

## Wiederverwendbarkeit vor Optimierung

Die Architektur soll zunächst möglichst viele zukünftige Berichte unterstützen.

Eine Optimierung einzelner Spezialfälle erfolgt nur bei Bedarf.

---

# 3. Zielbild

## Gesamtarchitektur

```text
Frontend
    ↓
Reporting API
    ↓
Reporting Services
    ↓
Fachliche Datenaufbereitung
    ↓
Ergebnisdaten
    ├── Tabellenansicht
    ├── PDF
    ├── Excel
    └── CSV
```

---

# 4. Architektur-Schichten

## Frontend

Verantwortung:

- Bericht auswählen
- Filter setzen
- Ergebnis anzeigen
- Export starten

Das Frontend enthält keine fachliche Reportinglogik.

---

## Reporting API

Verantwortung:

- Entgegennahme von Filterparametern
- Aufruf der Reporting Services
- Bereitstellung der Ergebnisse
- Bereitstellung von Exporten

Die Reporting API bildet die Schnittstelle zwischen Frontend und Backend.

---

## Reporting Services

Verantwortung:

- fachliche Auswertungen
- Datenselektion
- Sortierung
- Gruppierung
- Datenaufbereitung

Beispiele:

```text
MemberReportService

BirthdayReportService

MembershipReportService

StatisticsReportService
```

---

## Exportschicht

Verantwortung:

Erzeugung von:

```text
PDF

Excel

CSV
```

Die Exportschicht erhält bereits fachlich aufbereitete Daten.

Sie enthält keine eigene Fachlogik.

---

# 5. Gemeinsames Reporting-Modell

## Grundschema

Jeder Bericht folgt künftig demselben Ablauf.

```text
Bericht auswählen
        ↓
Filter setzen
        ↓
Auswertung erzeugen
        ↓
Ergebnis anzeigen
        ↓
optional exportieren
```

---

## Vorteile

- einheitliche Benutzerführung
- geringerer Entwicklungsaufwand
- bessere Wartbarkeit
- konsistente Berechtigungen

---

# 6. Filterarchitektur

## Ziel

Filter sollen nicht je Bericht neu entwickelt werden.

Stattdessen entsteht ein gemeinsames Filtermodell.

---

## Standardfilter

Mögliche wiederverwendbare Filter:

```text
Mitgliedsstatus

Stimme

Kammerchor

Zeitraum

Mitgliedsnummer

Name
```

---

## Berichtsspezifische Filter

Zusätzliche Spezialfilter bleiben möglich.

Beispiele:

```text
Geburtstagsmonat

Mindestalter

Zugehörigkeitsjahre
```

---

## Architekturprinzip

```text
Standardfilter
+
Berichtsspezifische Filter
=
Reportkonfiguration
```

---

# 7. Ergebnisdarstellung

## Primäre Darstellung

Ergebnisse werden zunächst im Frontend angezeigt.

Standard:

```text
Tabellenansicht
```

---

## Erweiterungen

Später möglich:

```text
Gruppierte Tabellen

Kartenansichten

Diagramme
```

Die Architektur soll diese Erweiterungen unterstützen.

---

# 8. Exportarchitektur

## Unterstützte Formate

Für die erste Reportinggeneration werden vorgesehen:

```text
PDF

Excel

CSV
```

---

## Prinzip

Alle Exportformate greifen auf dieselben Ergebnisdaten zu.

```text
Reporting Service
        ↓
Ergebnisdaten
        ↓
PDF

Excel

CSV
```

Dadurch wird doppelte Fachlogik vermieden.

---

# 9. Rollenintegration

Die Reportingarchitektur integriert sich vollständig in das bestehende Rollenmodell.

Vorhandene Rollen:

```text
VIEWER

EDITOR

ADMIN
```

Die eigentlichen Berechtigungen werden in einem separaten Berechtigungskonzept definiert.

---

# 10. Erweiterbarkeit

Die Architektur soll zukünftige Erweiterungen ermöglichen.

Beispiele:

```text
Dashboards

Ehrungsauswertungen

Finanzauswertungen

Dokumentgenerierung
```

ohne grundlegende Architekturänderungen.

---

# 11. Abgrenzung

Nicht Bestandteil dieser Architektur sind:

```text
konkrete Berichte

konkrete PDF-Layouts

konkrete Excel-Layouts

konkrete Exportbibliotheken
```

Diese Themen werden in den Folgephasen behandelt.

---

# 12. Zusammenfassung

Die zukünftige Reportingarchitektur basiert auf vier Grundprinzipien:

```text
Gemeinsame Reporting-API

Zentrale Reporting-Services

Gemeinsames Filtermodell

Gemeinsame Exportarchitektur
```

Dadurch entsteht eine einheitliche und erweiterbare Grundlage für alle zukünftigen Reportingfunktionen der EMC Mitgliederverwaltung.

Die Architektur dient als Referenz für:

- Backend-Konzept
- Frontend-Konzept
- Exportstrategie
- Berechtigungskonzept
- nachfolgende Reportingphasen
