# BL-020 Phase 1 – Berechtigungskonzept

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 1 – Reporting-Grundgerüst

---

# 1. Zielsetzung

Dieses Dokument definiert das Berechtigungskonzept für die zukünftigen Reportingfunktionen der EMC Mitgliederverwaltung.

Ziel ist die Festlegung einer einfachen, sicheren und datenschutzgerechten Lösung für die erste Reportinggeneration.

Das Konzept berücksichtigt insbesondere:

* personenbezogene Mitgliederdaten
* Exportfunktionen
* Listenbildung
* Datenschutzanforderungen
* zukünftige Erweiterbarkeit

---

# 2. Ausgangssituation

Die EMC Mitgliederverwaltung verwendet aktuell folgende Rollen:

```text
VIEWER

EDITOR

ADMIN
```

Diese Rollen wurden primär für die operative Mitgliederverwaltung entwickelt.

Beispiele:

```text
Mitglieder anzeigen

Mitglieder bearbeiten

Benutzer verwalten
```

Mit Einführung des Reportings entstehen zusätzliche Anforderungen.

---

# 3. Besonderheiten von Reporting

## Reporting ist nicht gleich Datenanzeige

Die Anzeige eines einzelnen Mitglieds unterscheidet sich fachlich und datenschutzrechtlich von einer Auswertung großer Datenmengen.

Beispiele:

```text
Mitglied anzeigen
```

gegen

```text
komplettes Mitgliederverzeichnis erzeugen
```

oder

```text
Geburtstagsliste exportieren
```

oder

```text
Excel-Datei mit Kontaktdaten erzeugen
```

---

## Datenschutzrelevanz

Reporting ermöglicht:

```text
Listenbildung

Massenanzeige

Massenauswertung

Datenexport
```

Dadurch entstehen höhere Datenschutzanforderungen als bei der Anzeige einzelner Datensätze.

---

## Exportfunktionen

Besonders kritisch sind:

```text
Excel

CSV

PDF
```

da diese Daten dauerhaft außerhalb der Anwendung gespeichert oder weitergegeben werden können.

---

# 4. Architekturgrundsatz

Für die erste Reportinggeneration wird ein konservativer Sicherheitsansatz verfolgt.

Grundsatz:

```text
erst sicher und einfach

später differenzieren
```

Nicht Ziel von Phase 1 ist die Einführung eines komplexen Berechtigungsmodells.

---

# 5. Phase-1-Berechtigungsmodell

## Entscheidung

Für die erste Reportinggeneration werden Reportingfunktionen ausschließlich Administratoren bereitgestellt.

```text
ADMIN
→ Reporting erlaubt

EDITOR
→ Reporting nicht erlaubt

VIEWER
→ Reporting nicht erlaubt
```

---

## Begründung

Vorteile:

```text
einfache Umsetzung

geringes Risiko

hohe Datensicherheit

keine zusätzlichen Rollen erforderlich

klare Verantwortlichkeit
```

---

## Auswirkungen

### Hauptnavigation

Nur Administratoren sehen:

```text
Reporting
```

als Hauptmenüpunkt.

---

### Routing

Nur Administratoren erhalten Zugriff auf:

```text
/reports

/reports/members

/reports/birthdays

/reports/memberships

/reports/control

/reports/statistics
```

---

### Backend

Alle Reporting-Endpunkte werden zusätzlich serverseitig abgesichert.

Frontend-Sichtbarkeit allein genügt nicht.

---

# 6. Berechtigungsmatrix Phase 1

## Mitgliederverwaltung

| Funktion              | VIEWER | EDITOR | ADMIN |
| --------------------- | ------ | ------ | ----- |
| Mitglieder anzeigen   | Ja     | Ja     | Ja    |
| Mitglieder bearbeiten | Nein   | Ja     | Ja    |
| Mitglieder anlegen    | Nein   | Ja     | Ja    |
| Mitglieder löschen    | Nein   | Nein   | Ja    |

---

## Reporting

| Funktion                | VIEWER | EDITOR | ADMIN |
| ----------------------- | ------ | ------ | ----- |
| Reporting-Menü sichtbar | Nein   | Nein   | Ja    |
| Berichte anzeigen       | Nein   | Nein   | Ja    |
| Berichte filtern        | Nein   | Nein   | Ja    |
| Berichte exportieren    | Nein   | Nein   | Ja    |

---

## Administration

| Funktion            | VIEWER | EDITOR | ADMIN |
| ------------------- | ------ | ------ | ----- |
| Benutzerverwaltung  | Nein   | Nein   | Ja    |
| Systeminformationen | Nein   | Nein   | Ja    |

---

# 7. Frontend-Umsetzung

## Menüanzeige

Empfohlen:

```text
ADMIN
→ Mitglieder
→ Reporting
→ Administration
```

```text
EDITOR
→ Mitglieder
```

```text
VIEWER
→ Mitglieder
```

---

## Zugriffsschutz

Nicht berechtigte Benutzer sollen:

```text
Reporting-Menü nicht sehen
```

und

```text
Reporting-Routen nicht aufrufen können
```

---

## Fehlerfall

Beim direkten Aufruf einer Reporting-URL:

```text
/reports
```

ohne Berechtigung:

```text
403 Forbidden
```

---

# 8. Backend-Umsetzung

## Grundsatz

Das Backend bleibt führend.

Alle Reporting-Endpunkte müssen serverseitig geschützt werden.

Beispiele:

```text
@PreAuthorize(...)

Security-Konstanten

ReportingPermissionService
```

---

## Nicht ausreichend

Nicht ausreichend ist:

```text
Frontend blendet Menü aus
```

Das Backend muss jeden Zugriff unabhängig prüfen.

---

# 9. Exportberechtigungen

## Phase 1

Exportfunktionen erhalten dieselben Berechtigungen wie Reporting.

```text
ADMIN
→ Export erlaubt

EDITOR
→ Export nicht erlaubt

VIEWER
→ Export nicht erlaubt
```

---

## Begründung

Exportierte Dateien verlassen die Anwendung.

Dadurch entstehen zusätzliche Datenschutzrisiken.

Die Freigabe erfolgt daher zunächst ausschließlich für Administratoren.

---

# 10. Zukünftige Erweiterungen

## Problemstellung

Die bestehenden Rollen:

```text
VIEWER

EDITOR

ADMIN
```

sind primär für die operative Mitgliederverwaltung konzipiert.

Für Reporting könnten sie langfristig zu grob sein.

---

## Mögliche Erweiterung

Spätere zusätzliche Berechtigungen:

```text
ROLE_REPORTING

ROLE_REPORT_EXPORT
```

oder

```text
REPORT_VIEW

REPORT_EXPORT
```

oder vergleichbare Konzepte.

---

## Mögliche spätere Matrix

Beispiel:

```text
EDITOR
→ Berichte anzeigen

ADMIN
→ Berichte anzeigen + exportieren
```

Dies ist jedoch ausdrücklich nicht Bestandteil von Phase 1.

---

# 11. Datenschutzbetrachtung

Die gewählte Lösung minimiert Risiken durch:

```text
begrenzten Benutzerkreis

klare Verantwortlichkeiten

keine Massenexports für Standardbenutzer
```

Dadurch wird die Einführung des Reportings vereinfacht und gleichzeitig datenschutzfreundlich gestaltet.

---

# 12. Teststrategie

Zu prüfen sind insbesondere:

```text
Reporting-Menü nur für ADMIN

Reporting-Routen nur für ADMIN

Reporting-API nur für ADMIN

Export-Endpunkte nur für ADMIN
```

Zusätzlich:

```text
403 Forbidden bei unberechtigtem Zugriff
```

---

# 13. Architekturentscheidung

## E-027 Reporting Startberechtigung

Für die erste Reportinggeneration werden sämtliche Reportingfunktionen ausschließlich Benutzern mit der Rolle ADMIN bereitgestellt.

Dies umfasst:

```text
Berichtsauswahl

Berichtsanzeige

Filterung

PDF-Export

Excel-Export

CSV-Export
```

Eine spätere Differenzierung der Berechtigungen bleibt ausdrücklich vorgesehen.

---

# 14. Zusammenfassung

Für Phase 1 wird ein bewusst konservatives Berechtigungsmodell eingeführt.

Grundsatz:

```text
Reporting
=
ADMIN
```

Dadurch entsteht:

```text
einfache Umsetzung

hohe Datensicherheit

klare Verantwortlichkeit

geringes Projektrisiko
```

Die bestehende Rollenstruktur bleibt unverändert.

Eine spätere Erweiterung um spezifische Reporting-Berechtigungen wird vorbereitet, ist jedoch nicht Bestandteil von BL-020 Phase 1.
