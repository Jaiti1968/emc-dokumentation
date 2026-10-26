# BL-020 Phase 1 – Reporting-Grundgerüst – Ergebnisdokument

## Status

Abgeschlossen

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 1 – Reporting-Grundgerüst

---

# 1. Ziel der Phase

Ziel von BL-020 Phase 1 war die fachliche und technische Vorbereitung des zukünftigen Reporting-Subsystems der EMC Mitgliederverwaltung.

Im Mittelpunkt stand ausdrücklich nicht die Umsetzung konkreter Berichte.

Ziel war die Schaffung eines belastbaren Grundgerüsts für:

* Navigation
* Architektur
* Berechtigungen
* Exportfunktionen
* spätere Reportingphasen

Die Phase bildet die Grundlage für die zukünftige Ablösung der Access-basierten Berichte.

---

# 2. Ausgangssituation

Zum Beginn der Phase existierte bereits eine fachliche Reportingstrategie aus BL-012.

Konkrete Reportingfunktionen waren jedoch noch nicht umgesetzt.

Offene Fragestellungen waren insbesondere:

```text
Welche Reportfamilien entstehen?

Wie wird Reporting in die Anwendung integriert?

Wie sieht die Frontend-Architektur aus?

Wie sieht die Backend-Architektur aus?

Wie werden Exporte erzeugt?

Wie werden Berechtigungen geregelt?
```

---

# 3. Erarbeitete Ergebnisse

Im Rahmen der Phase wurden folgende Architektur- und Konzeptdokumente erstellt:

```text
BL-020-phase-1-Fachliche-Kategorisierung-der-Reportingstruktur.md

BL-020-phase-1-Navigations-und-Menuestruktur.md

BL-020-phase-1-Reporting-Zielarchitektur.md

BL-020-phase-1-Backend-Konzept.md

BL-020-phase-1-Frontend-Konzept.md

BL-020-phase-1-Exportstrategie.md

BL-020-phase-1-Berechtigungskonzept.md
```

Diese Dokumente bilden gemeinsam die fachliche und technische Grundlage für alle nachfolgenden Reportingphasen.

---

# 4. Fachliche Zielstruktur

Im Rahmen der Analyse wurden folgende Reportfamilien definiert:

```text
Mitgliederwesen

Geburtstagswesen

Zugehörigkeitswesen

Kontrollberichte

Statistik
```

Zusätzlich wurde festgelegt:

```text
Mitgliederdatenblatt
```

gehört fachlich zur Reportfamilie:

```text
Mitgliederwesen
```

---

## Nicht Bestandteil des Reportings

Die Nutzungsvereinbarung Chorkleidung wird nicht als Reportingfunktion betrachtet.

Sie verbleibt im jeweiligen Fachprozess:

```text
Mitglied
↓
Chorkleidung
↓
Nutzungsvereinbarung erzeugen
```

---

# 5. Navigationskonzept

Für die zukünftige Anwendung wurde folgende Hauptnavigation definiert:

```text
Mitglieder

Reporting

Administration
```

Reporting wird als eigenständiger Fachbereich eingeführt.

Die bestehende Mitgliederverwaltung bleibt unverändert der operative Hauptbereich der Anwendung.

---

## Reporting-Menü

Vorgesehene Bereiche:

```text
Mitglieder

Geburtstage

Zugehörigkeiten

Kontrollberichte

Statistik
```

Diese Struktur bildet die Grundlage für alle zukünftigen Berichte.

---

# 6. Zielarchitektur

Die Reportingarchitektur folgt einem gemeinsamen Modell.

```text
Frontend
        ↓
Reporting API
        ↓
Reporting Services
        ↓
Ergebnisdaten
        ↓
Anzeige / Export
```

Wesentliche Architekturprinzipien:

```text
gemeinsame API

gemeinsame Services

gemeinsame Filter

gemeinsame Exportarchitektur
```

---

# 7. Backend-Konzept

Für Reporting wurde ein eigenständiger Backend-Bereich definiert.

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

## Langfristige Backend-Zielstruktur

Zusätzlich wurde eine langfristige fachbereichsorientierte Backend-Struktur definiert.

```text
common

member

reporting

admin

auth

lookup
```

---

## Architekturentscheidung

Die beschriebene Zielstruktur stellt ein langfristiges Architekturziel dar.

BL-020 Phase 1 führt zunächst ausschließlich den neuen Fachbereich:

```text
reporting
```

ein.

Eine vollständige Umstrukturierung des Backends ist nicht Bestandteil dieser Phase.

---

# 8. Frontend-Konzept

Das Frontend erhält erstmals einen eigenständigen Reporting-Bereich.

Wesentliche Bausteine:

```text
ReportingOverviewPage

ReportFilterPanel

ReportResultTable

ReportActions

reportApi.js
```

Zusätzlich wurde eine zukünftige Routingstruktur definiert.

---

## Frontend-Grundprinzip

Jeder Bericht folgt demselben Bedienmodell:

```text
Bericht auswählen
        ↓
Filter setzen
        ↓
Vorschau anzeigen
        ↓
optional exportieren
```

---

# 9. Exportstrategie

Für die erste Reportinggeneration wurden folgende Exportformate definiert:

```text
PDF

Excel

CSV
```

---

## Grundsatz

Die Vorschau innerhalb der Anwendung bleibt führend.

Prinzip:

```text
Vorschau
        ↓
Export
```

Nicht:

```text
Filter
        ↓
direkt PDF
```

---

## Weitere Entscheidung

Es wird bewusst keine externe Reportingplattform eingesetzt.

Nicht vorgesehen:

```text
JasperReports

BIRT

Crystal Reports
```

Die Exportfunktionen werden vollständig innerhalb der bestehenden Anwendung umgesetzt.

---

# 10. Berechtigungskonzept

Im Rahmen der Analyse wurde festgestellt, dass Reporting höhere Datenschutzanforderungen besitzt als die Anzeige einzelner Mitgliederdaten.

Gründe:

```text
Listenbildung

Massenauswertungen

Datenexporte

personenbezogene Daten
```

---

# 11. Architekturentscheidungen

## E-027 Reporting Startberechtigung

Für die erste Reportinggeneration werden sämtliche Reportingfunktionen ausschließlich Benutzern mit der Rolle:

```text
ADMIN
```

bereitgestellt.

Dies umfasst:

```text
Berichtsauswahl

Berichtsanzeige

Filterung

PDF-Export

Excel-Export

CSV-Export
```

Eine spätere Differenzierung bleibt ausdrücklich vorgesehen.

---

## E-028 Reporting als eigenständiger Fachbereich

Reporting wird als eigenständiger Fachbereich der Anwendung eingeführt.

Zielstruktur:

```text
Mitglieder

Reporting

Administration
```

Reporting wird damit fachlich, technisch und organisatorisch von der operativen Mitgliederverwaltung getrennt.

---

# 12. Datenschutzbewertung

Die gewählte Architektur berücksichtigt den datenschutzrechtlich sensiblen Charakter der Mitgliederdaten.

Insbesondere:

```text
Mitgliederverzeichnisse

Geburtstagslisten

Kontaktdaten

Exportdateien
```

werden durch die gewählte Berechtigungsstrategie zunächst ausschließlich Administratoren zugänglich gemacht.

Dadurch wird die Einführung des Reportings mit geringem Risiko ermöglicht.

---

# 13. Nicht Bestandteil von Phase 1

Bewusst nicht umgesetzt wurden:

```text
Mitgliederverzeichnis

Geburtstagsliste

Zugehörigkeitsauswertung

Kontrollberichte

Statistikberichte

PDF-Layouts

Excel-Layouts

CSV-Strukturen
```

Diese Themen werden in den nachfolgenden Reportingphasen behandelt.

---

# 14. Vorbereitung der Folgephase

Die nächste Umsetzungsphase ist:

```text
BL-020 Phase 2 – Mitgliederverzeichnis
```

Die hierfür notwendigen Grundlagen wurden in Phase 1 vollständig geschaffen.

---

# 15. Dokumentationsauswirkungen

Neu entstanden:

```text
BL-020-phase-1-Fachliche-Kategorisierung-der-Reportingstruktur.md

BL-020-phase-1-Navigations-und-Menuestruktur.md

BL-020-phase-1-Reporting-Zielarchitektur.md

BL-020-phase-1-Backend-Konzept.md

BL-020-phase-1-Frontend-Konzept.md

BL-020-phase-1-Exportstrategie.md

BL-020-phase-1-Berechtigungskonzept.md
```

Zusätzlich entstanden die Architekturentscheidungen:

```text
E-027 Reporting Startberechtigung

E-028 Reporting als eigenständiger Fachbereich
```

---

# 16. Gesamtbewertung

BL-020 Phase 1 wurde erfolgreich abgeschlossen.

Die Phase hat ein vollständiges fachliches und technisches Grundgerüst für das zukünftige Reporting geschaffen.

Definiert wurden:

```text
Reportfamilien

Navigation

Frontend-Architektur

Backend-Architektur

Exportstrategie

Berechtigungskonzept
```

Damit verfügt die EMC Mitgliederverwaltung erstmals über eine konsistente Reporting-Zielarchitektur.

Die Voraussetzungen für die Umsetzung konkreter Berichte sind geschaffen.

---

# 17. Abschlussstatus

BL-020 Phase 1 wird als erfolgreich abgeschlossen bewertet.

Alle definierten Ziele der Phase wurden erreicht.

Die Umsetzung konkreter Reportingfunktionen erfolgt in den nachfolgenden Phasen.

Nächste geplante Phase:

```text
BL-020 Phase 2 – Mitgliederverzeichnis
```
