# BL-020 Phase 2 – Mitgliederverzeichnis – Backend-Konzept

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 2 – Mitgliederverzeichnis

---

# 1. Zielsetzung

Dieses Dokument beschreibt die Backend-Architektur für den ersten produktiven Reporting-Baustein:

```text
Mitgliederverzeichnis
```

Ziel ist die Definition einer wiederverwendbaren Reporting-Architektur, die später auch für:

- Geburtstagswesen
- Zugehörigkeitswesen
- Ehrungswesen
- Kontrollberichte
- Statistik

verwendet werden kann.

Dabei sollen die in BL-020 Phase 1 definierten Architekturprinzipien umgesetzt werden:

```text
Fachlogik
↓
Reporting-Service
↓
Ausgabeformat
```

Die fachliche Berechnung erfolgt einmalig im Backend und wird anschließend von verschiedenen Ausgabeformaten genutzt.

---

# 2. Architekturprinzipien

## Zentrale Fachlogik

Die fachliche Ermittlung des Mitgliederverzeichnisses erfolgt ausschließlich im Backend.

Das Frontend enthält keine Berichtslogik.

---

## Trennung von Daten und Darstellung

Das Backend liefert ausschließlich fachlich aufbereitete Daten.

Die Darstellung erfolgt anschließend durch:

```text
Browser-Vorschau

PDF

Excel

CSV
```

---

## Wiederverwendbarkeit

Die Datenbasis für:

```text
Vorschau
PDF
Excel
CSV
```

muss identisch sein.

Die fachliche Logik darf nicht mehrfach implementiert werden.

---

## Erweiterbarkeit

Die Architektur soll weitere Reports unterstützen können.

Daher wird bereits in Phase 2 eine generische Reporting-Struktur vorbereitet.

---

# 3. Schichtenmodell

## Zielarchitektur

```text
Controller
      ↓
Reporting Service
      ↓
Repository Layer
      ↓
Datenbank
```

---

## Verantwortlichkeiten

### Controller

- Request entgegennehmen
- Berechtigungen prüfen
- DTOs validieren
- Service aufrufen
- Ergebnis zurückgeben

---

### Reporting Service

- Filter anwenden
- Gruppierungen vorbereiten
- Sortierungen durchführen
- Berichtsdaten erzeugen

---

### Repository Layer

- Datenbankzugriffe
- Suchabfragen
- Filterabfragen

Keine Reportinglogik.

---

# 4. API-Endpunkte

## Bericht anzeigen

```http
POST /api/reporting/members-directory/query
```

Zweck:

Erzeugung der Browser-Vorschau.

---

## PDF Export

```http
POST /api/reporting/members-directory/export/pdf
```

Zweck:

Erzeugung einer PDF-Datei.

---

## Excel Export

```http
POST /api/reporting/members-directory/export/excel
```

Zweck:

Erzeugung einer Excel-Datei.

---

## CSV Export (optional vorbereitet)

```http
POST /api/reporting/members-directory/export/csv
```

CSV kann bereits architektonisch vorgesehen werden, muss aber nicht zwingend in Phase 2 umgesetzt werden.

---

# 5. Berechtigungen

## Aktuelle Phase

Zugriff:

```text
ADMIN
```

---

## Prüfung

Die Berechtigungsprüfung erfolgt serverseitig.

Nicht berechtigte Benutzer erhalten:

```http
403 Forbidden
```

---

# 6. Request DTO

## MembersDirectoryReportRequest

Zweck:

Übertragung aller Filterparameter für die Erzeugung des Mitgliederverzeichnisses.

Beispiel:

```json
{
  "variant": "STANDARD",
  "memberStatusIds": [1, 2],
  "voiceIds": [1, 2],
  "chamberChoirMode": "ALL",
  "searchText": "Müller"
}
```

---

## Variant

Technische Enum-Werte:

```text
STANDARD
COMPACT
CONTINUOUS
CHAMBER_CHOIR
```

Bedeutung:

```text
STANDARD
=
Standarddarstellung

COMPACT
=
Kompaktdarstellung

CONTINUOUS
=
Fortlaufende Darstellung

CHAMBER_CHOIR
=
Kammerchor-Variante
```

---

## ChamberChoirMode

Technische Enum-Werte:

```text
ALL
ONLY_CHAMBER_CHOIR
WITHOUT_CHAMBER_CHOIR
```

Bedeutung:

```text
ALL
=
Alle Mitglieder

ONLY_CHAMBER_CHOIR
=
Nur Kammerchor-Mitglieder

WITHOUT_CHAMBER_CHOIR
=
Ohne Kammerchor-Mitglieder
```

---

## Umgang mit technischen Enum-Werten und deutscher Oberfläche

Die API verwendet stabile technische Enum-Werte.

Diese Werte sind nicht für die Anzeige im Frontend bestimmt.

Das Frontend bildet technische Werte auf deutsche Anzeigetexte ab.

Dadurch bleiben API und Backend stabil, während die Benutzeroberfläche vollständig deutsch bleibt.

Beispiele:

```text
STANDARD        → Standard
COMPACT         → Kompakt
CONTINUOUS      → Fortlaufend
CHAMBER_CHOIR   → Kammerchor
```

sowie:

```text
ALL                     → Alle
ONLY_CHAMBER_CHOIR      → Nur Kammerchor
WITHOUT_CHAMBER_CHOIR   → Ohne Kammerchor
```

Technische Enum-Werte dürfen nicht ungefiltert in der Benutzeroberfläche angezeigt werden.

---

## Fachlicher Grundsatz

Die fachliche Bedeutung der Varianten wird ausschließlich durch die Reportinglogik bestimmt.

Die Enum-Werte dienen lediglich der technischen Steuerung von:

- Layoutvarianten
- Gruppierungsverhalten
- Kammerchor-Filterung

Sie enthalten keine fachlichen Berichtsdaten.

---

# 7. Ergebnis DTO

## MembersDirectoryReportResponse

Zweck:

Übertragung der erzeugten Berichtsdaten.

Beispiel:

```json
{
  "memberCount": 127,
  "groups": [...]
}
```

---

# 8. Gruppenmodell

## Ziel

Die Gruppierungslogik wird bereits im Backend vorbereitet.

Dadurch können:

```text
Browser
PDF
Excel
```

dieselbe Struktur verwenden.

---

## ReportGroupDto

Beispiel:

```json
{
  "groupName": "Aktive Mitglieder",
  "subGroups": [...]
}
```

---

## ReportSubGroupDto

Beispiel:

```json
{
  "groupName": "1. Tenor",
  "members": [...]
}
```

---

# 9. Mitgliedsdatensatz

## ReportMemberDto

Enthält die fachlichen Berichtsdaten.

Beispiel:

```json
{
  "memberNumber": "...",
  "lastName": "...",
  "firstName": "...",
  "city": "...",
  "email": "...",
  "voice": "1. Tenor"
}
```

---

## Verfügbare Felder

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

# 10. Fachliche Gruppierungslogik

## Grundsatz

Die Gruppierung erfolgt abhängig vom Mitgliedsstatus.

---

## Aktive Mitglieder

Gruppierung:

```text
Mitgliedsstatus
↓
Stimme
```

Beispiel:

```text
Aktive Mitglieder
  1. Tenor
  2. Tenor
  1. Bass
  2. Bass
```

---

## Andere Mitgliedsstatus

Gruppierung:

```text
Mitgliedsstatus
```

Keine zusätzliche Stimmgruppierung.

---

## Kammerchor

Gruppierung:

```text
Stimme
```

Da Kammerchor-Mitglieder ausschließlich aktive Mitglieder sein können, entfällt die Statusgruppe.

---

# 11. Sortierungslogik

## Einheitliche Sortierung

Innerhalb jeder Gruppe erfolgt die Sortierung nach:

```text
Nachname
↓
Vorname
```

Dies entspricht dem bisherigen Access-Verhalten.

---

# 12. Reporting Service

## MembersDirectoryReportService

Zentrale fachliche Komponente.

Verantwortlich für:

- Datenbeschaffung
- Filterung
- Gruppierung
- Sortierung
- Ergebnisaufbereitung

---

## Hauptmethode

Beispiel:

```java
MembersDirectoryReportResponse createReport(
    MembersDirectoryReportRequest request
)
```

---

# 13. Datenzugriff

## MemberRepository

Bestehende Repository-Strukturen sollen soweit möglich wiederverwendet werden.

---

## Suchstrategie

Empfohlen:

```text
Datenbankfilterung
↓
fachliche Aufbereitung im Service
```

Nicht empfohlen:

```text
Alle Mitglieder laden
↓
vollständige Filterung im Speicher
```

---

# 14. Exportarchitektur

## Grundprinzip

Die Exporte greifen auf dieselbe Berichtsstruktur zu wie die Browser-Vorschau.

```text
MembersDirectoryReportService
            ↓
MembersDirectoryReportResponse
            ↓
PDF
Excel
CSV
```

---

## Vorteil

Fachliche Logik existiert nur einmal.

Fehlerquellen werden reduziert.

---

# 15. PDF-Erzeugung

## Phase 2

Empfohlen:

Eigenständige PDF-Export-Komponente.

Beispiel:

```text
MembersDirectoryPdfExporter
```

Aufgabe:

Umwandlung der Berichtsstruktur in PDF.

Keine Fachlogik.

---

# 16. Excel-Erzeugung

## Phase 2

Empfohlen:

Eigenständige Export-Komponente.

Beispiel:

```text
MembersDirectoryExcelExporter
```

Aufgabe:

Erzeugung einer Excel-Datei aus derselben Berichtsstruktur.

Keine Fachlogik.

---

# 17. Fehlerbehandlung

## Ungültige Filter

Antwort:

```http
400 Bad Request
```

---

## Fehlende Berechtigung

Antwort:

```http
403 Forbidden
```

---

## Technische Fehler

Antwort:

```http
500 Internal Server Error
```

Die konkrete Fehlermeldung folgt den Regeln aus BL-014 Security Hardening.

---

# 18. Teststrategie

## Service Tests

Absicherung von:

- Filterung
- Gruppierung
- Sortierung
- Kammerchor-Logik

---

## Controller Tests

Absicherung von:

- Berechtigungen
- Request-Validierung
- API-Verhalten

---

## Integrationstests

Absicherung von:

- Datenbankzugriff
- vollständiger Berichtserzeugung

---

# 19. Wiederverwendung für zukünftige Reports

Die Architektur soll später erweitert werden können auf:

```text
BirthdayReportService

MembershipReportService

HonorReportService

StatisticsReportService
```

Dadurch entsteht eine einheitliche Reporting-Landschaft.

---

# 20. Technische Entscheidungsempfehlung

Für Phase 2 wird empfohlen:

```text
ReportingController
↓
MembersDirectoryReportService
↓
MembersDirectoryReportResponse
↓
PDF / Excel / CSV
```

Die Browser-Vorschau und alle Exportformate greifen auf dieselbe Berichtsstruktur zu.

Damit wird die in BL-012 und BL-020 definierte Reporting-Strategie konsequent umgesetzt.

---

# 21. Abschlussbewertung

Das Backend-Konzept schafft eine klare Trennung zwischen:

```text
Fachlogik
↓
Berichtsstruktur
↓
Ausgabeformat
```

Die Lösung ist:

- einfach umsetzbar
- wartbar
- testbar
- wiederverwendbar

und bildet die technische Grundlage für die weitere Reporting-Migration der EMC Mitgliederverwaltung.
