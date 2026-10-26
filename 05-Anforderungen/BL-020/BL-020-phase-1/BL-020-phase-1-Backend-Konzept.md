# BL-020 Phase 1 – Backend-Konzept

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 1 – Reporting-Grundgerüst

---

# 1. Zielsetzung

Dieses Dokument definiert das Backend-Konzept für das zukünftige Reporting der EMC Mitgliederverwaltung.

Ziel ist die Schaffung einer konsistenten Backend-Architektur für:

- Reporting
- Exportfunktionen
- Filtermechanismen
- zukünftige Berichtsfamilien

Gleichzeitig wird eine langfristige Backend-Zielstruktur beschrieben, welche die bestehende Mitgliederverwaltung und das zukünftige Reporting sauber voneinander trennt.

---

# 2. Architekturgrundsätze

## Trennung von operativer Verarbeitung und Reporting

Die EMC Mitgliederverwaltung unterscheidet künftig zwischen:

```text
Mitgliederverwaltung
=
Pflege und Verwaltung von Daten

Reporting
=
Auswertung und Darstellung von Daten
```

Dadurch bleiben operative Funktionen und Auswertungsfunktionen unabhängig voneinander.

---

## Fachlogik nur einmal implementieren

Fachliche Regeln werden ausschließlich im Backend umgesetzt.

Beispiele:

- Altersberechnungen
- Geburtstagslogik
- Zugehörigkeitsjahre
- Jubiläumslogik

Die Fachlogik darf nicht mehrfach für:

- Frontend
- PDF
- Excel
- CSV

implementiert werden.

---

## Reporting ist grundsätzlich lesend

Reporting dient ausschließlich der Auswertung.

Reporting-Komponenten verändern keine Fachdaten.

Schreibende Operationen verbleiben in den jeweiligen Fachmodulen.

---

## Gemeinsame Infrastruktur

Technische Bausteine, die von mehreren Fachbereichen benötigt werden, werden zentral bereitgestellt.

Fachliche Logik verbleibt im jeweiligen Fachbereich.

---

# 3. Vorgeschlagene Backend-Gesamtstruktur

Die bestehende Backend-Struktur ist aktuell überwiegend technisch organisiert.

Mit Einführung des Reportings wird eine stärkere fachliche Gliederung empfohlen.

---

## Zielstruktur

```text
de.emc.mitglieder
├── common
│   ├── dto
│   ├── error
│   ├── exception
│   ├── logging
│   ├── mapper
│   └── validation
│
├── config
├── constant
│
├── member
│   ├── controller
│   ├── dto
│   ├── mapper
│   ├── repository
│   └── service
│
├── reporting
│   ├── controller
│   ├── dto
│   ├── mapper
│   ├── repository
│   ├── service
│   ├── export
│   └── security
│
├── admin
│   ├── controller
│   ├── dto
│   ├── repository
│   └── service
│
├── auth
│   ├── controller
│   ├── dto
│   └── service
│
├── lookup
│   ├── dto
│   ├── repository
│   └── service
│
├── security
│
└── MitgliederBackendApplication
```

---

# 4. Fachliche Bereiche

## common

Der Bereich `common` enthält ausschließlich technische Komponenten, die fachbereichsübergreifend genutzt werden.

Beispiele:

```text
ErrorResponseDto

PaginationDto

ValidationResultDto

Exception-Klassen

technische Mapper-Hilfen

Logging-Hilfen
```

Nicht in `common` gehören:

- fachliche DTOs
- fachliche Services
- Fachlogik

---

## member

Der Bereich `member` enthält die operative Mitgliederverwaltung.

Verantwortung:

```text
Mitgliederliste

Mitgliederdetail

Stammdatenpflege

Kontaktpflege

Mitgliedschaftspflege
```

Beispiele:

```text
MemberController

MemberService

MemberRepository

MemberMapper
```

---

## reporting

Der Bereich `reporting` enthält sämtliche Auswertungs- und Berichtsfunktionen.

Verantwortung:

```text
Mitgliederverzeichnis

Geburtstagslisten

Zugehörigkeitsauswertungen

Kontrollberichte

Statistiken
```

Reporting arbeitet grundsätzlich lesend.

---

## admin

Der Bereich `admin` enthält Verwaltungsfunktionen.

Verantwortung:

```text
Benutzerverwaltung

Rollenverwaltung

Systemadministration
```

---

## auth

Der Bereich `auth` enthält Authentifizierungsfunktionen.

Verantwortung:

```text
Login

Logout

Session-Verwaltung
```

---

## lookup

Der Bereich `lookup` enthält fachübergreifende Nachschlagewerte.

Beispiele:

```text
Mitgliedsstatus

Stimmen

weitere Lookup-Werte
```

Diese Informationen werden sowohl von der Mitgliederverwaltung als auch vom Reporting verwendet.

Daher bleibt `lookup` als eigenständiger Bereich bestehen.

---

# 5. Reporting-Package-Struktur

## Zielstruktur

```text
reporting
├── controller
├── dto
├── mapper
├── repository
├── service
├── export
└── security
```

---

## controller

Verantwortung:

```text
REST-Endpunkte

Parameterannahme

Response-Erzeugung
```

Keine Fachlogik.

---

## service

Verantwortung:

```text
Datenselektion

Sortierung

Gruppierung

fachliche Berechnungen

Ergebnisaufbereitung
```

---

## dto

Verantwortung:

```text
Filterobjekte

Ergebnisobjekte

Exportobjekte
```

---

## mapper

Verantwortung:

```text
Entity
↓
Report DTO
```

---

## repository

Verantwortung:

```text
Reporting-spezifische Datenabfragen
```

Für komplexe Berichte dürfen spezialisierte Leseabfragen verwendet werden.

---

## export

Verantwortung:

```text
PDF

Excel

CSV
```

Keine Fachlogik.

---

## security

Verantwortung:

```text
Reporting-Berechtigungen

Reporting-Zugriffskontrolle
```

---

# 6. Controller-Konzept

## Basis-URL

Alle Reporting-Endpunkte werden unterhalb eines gemeinsamen API-Pfades bereitgestellt.

```text
/api/reports
```

---

## Reportfamilien

Beispiele:

```text
/api/reports/members

/api/reports/birthdays

/api/reports/memberships

/api/reports/control

/api/reports/statistics
```

---

## Controller

Empfohlene Struktur:

```text
MemberReportController

BirthdayReportController

MembershipReportController

ControlReportController

StatisticsReportController
```

---

## Nicht empfohlen

Die Erweiterung bestehender operativer Controller um Reportingfunktionen.

Beispiel:

```text
MemberController
```

soll kein Reporting übernehmen.

---

# 7. Service-Konzept

## Reportfamilien

Jede Reportfamilie erhält einen eigenen Service.

Beispiele:

```text
MemberReportService

BirthdayReportService

MembershipReportService

ControlReportService

StatisticsReportService
```

---

## Aufgaben

Reporting-Services übernehmen:

```text
Filterauswertung

Datenbeschaffung

fachliche Berechnungen

Sortierung

Gruppierung

DTO-Erzeugung
```

---

# 8. DTO-Konzept

## Filter-DTOs

Beispiele:

```text
MemberReportFilterDto

BirthdayReportFilterDto

MembershipReportFilterDto

ControlReportFilterDto

StatisticsReportFilterDto
```

---

## Gemeinsame Filterfelder

Wiederverwendbare Filter:

```text
search

statusId

voiceId

memberNumber

fromDate

toDate

sort

page

pageSize
```

---

## Ergebnis-DTOs

Beispiele:

```text
MemberReportRowDto

BirthdayReportRowDto

MembershipReportRowDto

ControlReportRowDto

StatisticsResultDto
```

---

## Gemeinsames Response-Modell

```text
ReportResultDto<T>
```

Inhalt:

```text
items

totalItems

page

pageSize

reportName

generatedAt

appliedFilters
```

---

# 9. API-Konzept

## Vorschau

Reporting liefert zunächst strukturierte JSON-Daten.

Beispiel:

```http
GET /api/reports/members
```

Antwort:

```text
ReportResultDto<MemberReportRowDto>
```

---

## Export

Separate Endpunkte:

```http
GET /api/reports/members/export/pdf

GET /api/reports/members/export/xlsx

GET /api/reports/members/export/csv
```

---

## Einheitliches Prinzip

```text
Vorschau
+
Export
=
gleiche fachliche Datenbasis
```

---

# 10. Exportarchitektur

## Grundprinzip

```text
ReportService
        ↓
ReportResultDto
        ↓
ExportService
        ↓
PDF

Excel

CSV
```

---

## Gemeinsame technische Exportservices

Beispiele:

```text
PdfExportService

ExcelExportService

CsvExportService
```

Diese Services enthalten keine Fachlogik.

---

# 11. Datenzugriff

Reporting nutzt dieselben Datenbestände wie die Mitgliederverwaltung.

Mögliche Zugriffsformen:

```text
Repository

Projection

spezialisierte Reporting-Queries
```

---

## Grundsatz

Für Reporting dürfen optimierte Leseabfragen verwendet werden.

Das Ziel ist:

```text
gute Lesbarkeit

gute Wartbarkeit

keine doppelte Fachlogik
```

---

# 12. Berechtigungen

Reporting integriert sich in das bestehende Rollenmodell.

Vorhandene Rollen:

```text
VIEWER

EDITOR

ADMIN
```

Die konkrete Berechtigungsmatrix wird in einem separaten Dokument definiert.

---

# 13. Fehlerbehandlung

Reporting verwendet die bestehende zentrale Fehlerbehandlung.

Beispiele:

```text
ungültige Filter

keine Berechtigung

Report nicht verfügbar

Exportformat nicht unterstützt
```

Technische Details werden nicht an Clients weitergegeben.

---

# 14. Teststrategie

Empfohlene Tests:

```text
Controller-Tests

Service-Tests

Security-Tests

Export-Tests
```

Der konkrete Umfang wird je Reportfamilie festgelegt.

---

# 15. Migrationsempfehlung

Die bestehende Backend-Struktur muss nicht sofort vollständig umgebaut werden.

Für BL-020 Phase 1 genügt zunächst:

```text
Einführung des Bereichs

reporting
```

Die langfristige Zielstruktur dient als Orientierung für zukünftige Erweiterungen und Refactorings.

## Architekturentscheidung

Die in diesem Dokument beschriebene fachliche Zielstruktur stellt ein langfristiges Architekturziel dar.

Für die Umsetzung von BL-020 Phase 1 wird lediglich die Einführung eines neuen fachlichen Bereichs **reporting** vorgesehen.

Die bestehende Backend-Struktur für Mitgliederverwaltung, Authentifizierung und Administration bleibt zunächst unverändert.

Eine vollständige fachbereichsorientierte Umstrukturierung des Backends ist nicht Bestandteil von BL-020 und wird gegebenenfalls als separates Architekturvorhaben behandelt.

---

# 16. Zusammenfassung

Die zukünftige Backend-Architektur trennt:

```text
member

reporting

admin

auth

lookup

common
```

voneinander.

Dadurch entsteht eine fachlich strukturierte und langfristig erweiterbare Backend-Architektur.

Für das Reporting werden eingeführt:

```text
Reporting Controller

Reporting Services

Filter DTOs

Result DTOs

Export Services

Reporting Security
```

Diese Architektur bildet die Grundlage für alle zukünftigen Reportingfunktionen der EMC Mitgliederverwaltung.
