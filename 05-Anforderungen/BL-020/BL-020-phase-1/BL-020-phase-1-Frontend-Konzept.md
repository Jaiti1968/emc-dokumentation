# BL-020 Phase 1 – Frontend-Konzept

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 1 – Reporting-Grundgerüst

---

# 1. Zielsetzung

Dieses Dokument definiert das Frontend-Konzept für das zukünftige Reporting der EMC Mitgliederverwaltung.

Ziel ist die Schaffung einer einheitlichen Benutzeroberfläche für:

- Reporting-Navigation
- Berichtsauswahl
- Filterung
- Ergebnisanzeige
- Exportfunktionen

Das Konzept bildet die Grundlage für die spätere Umsetzung des Reporting-Grundgerüsts und der nachfolgenden fachlichen Reportingphasen.

---

# 2. Ausgangssituation

Das bestehende Frontend ist bislang primär auf die operative Mitgliederverwaltung ausgerichtet.

Vorhandene zentrale Bereiche:

```text
Mitgliederliste

Mitgliedersuche

Mitgliederdetailansicht

Mitgliedspflege

Administration
```

Mit BL-020 wird erstmals ein eigenständiger Reporting-Bereich eingeführt.

Dadurch entsteht eine erweiterte Hauptnavigation mit mehreren fachlichen Bereichen.

Zielbild:

```text
Mitglieder

Reporting

Administration
```

---

# 3. Grundsätze

## Reporting als eigener Frontend-Bereich

Reporting wird als eigener Bereich im Frontend eingeführt.

Es wird nicht in die bestehende Mitgliederliste eingebettet.

Begründung:

- klare fachliche Trennung
- bessere Erweiterbarkeit
- bessere Orientierung für Benutzer
- Vorbereitung zukünftiger Auswertungen und Statistiken

---

## Bestehende Mitgliederfunktionen bleiben erhalten

Die vorhandene Mitgliederverwaltung bleibt der operative Hauptbereich der Anwendung.

BL-020 verändert nicht die fachliche Bedienlogik der Mitgliederpflege.

Bestehende Funktionen bleiben erhalten:

```text
Mitglieder suchen

Mitglieder filtern

Mitglied anzeigen

Mitglied bearbeiten
```

---

## Wiederverwendung vor Neuentwicklung

Bestehende Frontend-Konzepte sollen möglichst wiederverwendet werden.

Insbesondere:

```text
Suchfeld

Statusfilter

Stimmenfilter

Tabellen- oder Listenansicht

Ladezustände

Fehleranzeigen

Rollenabhängige Anzeige
```

werden als Grundlage für Reporting-Komponenten betrachtet.

---

# 4. Auswirkungen auf die Hauptnavigation

## Zielbild

Die Anwendung erhält eine dauerhafte Hauptnavigation.

```text
Mitglieder

Reporting

Administration
```

---

## Mitglieder

Operativer Bereich für:

```text
Mitgliederliste

Mitgliederdetail

Mitgliedspflege
```

---

## Reporting

Auswertungsbereich für:

```text
Mitgliederberichte

Geburtstagsberichte

Zugehörigkeitsberichte

Kontrollberichte

Statistik
```

---

## Administration

Administrativer Bereich für:

```text
Benutzerverwaltung

Systeminformationen

zukünftige Administrationsfunktionen
```

Die Sichtbarkeit administrativer Menüpunkte bleibt rollenabhängig.

---

# 5. Routing-Konzept

## Grundstruktur

Empfohlene Routen:

```text
/members

/members/:mitgliedsnummer

/reports

/reports/members

/reports/members/directory

/reports/members/datasheet

/reports/birthdays

/reports/memberships

/reports/control

/reports/statistics

/admin
```

---

## Startseite Reporting

Route:

```text
/reports
```

Zweck:

- Übersicht der verfügbaren Reportfamilien
- Einstieg in Mitgliederberichte, Geburtstage, Zugehörigkeiten, Kontrollberichte und Statistik
- spätere Anzeige zuletzt genutzter Berichte möglich

---

## Mitgliederberichte

Beispiele:

```text
/reports/members/directory

/reports/members/datasheet
```

---

## Geburtstagsberichte

Beispiel:

```text
/reports/birthdays
```

---

## Zugehörigkeitsberichte

Beispiel:

```text
/reports/memberships
```

---

## Kontrollberichte

Beispiel:

```text
/reports/control
```

---

## Statistik

Beispiel:

```text
/reports/statistics
```

---

# 6. Seitenstruktur

## ReportingOverviewPage

Route:

```text
/reports
```

Aufgabe:

- Anzeige der Reportfamilien
- Navigation zu konkreten Reports

---

## ReportFamilyPage

Optionaler Zwischenschritt für größere Reportfamilien.

Beispiel:

```text
/reports/members
```

Aufgabe:

- Anzeige aller Mitgliederberichte
- spätere Erweiterbarkeit

---

## ReportPage

Allgemeines Seitenmuster für konkrete Berichte.

Aufbau:

```text
Seitentitel

Beschreibung

Filterbereich

Aktionen

Ergebnisbereich
```

---

# 7. Komponentenstruktur

## Empfohlene neue Struktur

```text
src/
├── pages/
│   └── reporting/
│       ├── ReportingOverviewPage.jsx
│       ├── MemberReportsPage.jsx
│       ├── MemberDirectoryReportPage.jsx
│       ├── MemberDatasheetReportPage.jsx
│       ├── BirthdayReportPage.jsx
│       ├── MembershipReportPage.jsx
│       ├── ControlReportsPage.jsx
│       └── StatisticsReportPage.jsx
│
├── components/
│   └── reporting/
│       ├── ReportCard.jsx
│       ├── ReportFilterPanel.jsx
│       ├── ReportActions.jsx
│       ├── ReportResultTable.jsx
│       ├── ReportEmptyState.jsx
│       ├── ReportLoadingState.jsx
│       └── ReportErrorState.jsx
│
├── api/
│   └── reportApi.js
│
└── hooks/
    └── useReport.js
```

---

# 8. Gemeinsames Bedienmodell

Jeder Bericht soll nach demselben Muster bedient werden.

```text
Bericht auswählen
        ↓
Filter setzen
        ↓
Ergebnis anzeigen
        ↓
Export starten
```

---

## Standardseitenaufbau

```text
Titel

Kurzbeschreibung

Filterbereich

Button: Aktualisieren

Button: Filter zurücksetzen

Ergebnisbereich

Exportaktionen
```

---

# 9. Filterkonzept

## Wiederverwendbare Filter

Bestehende Filterkonzepte der Mitgliederliste werden als Grundlage verwendet.

Beispiele:

```text
Suche

Status

Stimme
```

---

## Berichtsspezifische Filter

Zusätzliche Filter können je Bericht ergänzt werden.

Beispiele:

```text
Zeitraum

Geburtstagsmonat

Zugehörigkeitsjahre

Kammerchor

Mitgliedsnummer
```

---

## ReportFilterPanel

`ReportFilterPanel` soll als wiederverwendbare Komponente dienen.

Es soll Standardfilter anzeigen können und bei Bedarf berichtsspezifisch erweitert werden.

Prinzip:

```text
Standardfilter

+

berichtsspezifische Filter
```

---

# 10. Ergebnisdarstellung

## Standard

Für die erste Reportinggeneration wird eine tabellarische Ergebnisdarstellung empfohlen.

```text
ReportResultTable
```

---

## Varianten

Später möglich:

```text
gruppierte Tabellen

Kartenansichten

Diagramme

Druckvorschau
```

---

## Mitgliederdatenblatt

Das Mitgliederdatenblatt ist ein Sonderfall innerhalb der Mitgliederberichte.

Es kann für:

- ein Mitglied
- mehrere Mitglieder
- eine gefilterte Mitgliedermenge

erzeugt werden.

Die Filterung erfolgt über dasselbe Grundmodell wie beim Mitgliederverzeichnis.

---

# 11. Exportaktionen

## ReportActions

Die Komponente `ReportActions` stellt exportbezogene Aktionen bereit.

Mögliche Aktionen:

```text
PDF

Excel

CSV

Drucken
```

Nicht jeder Bericht muss alle Aktionen anbieten.

Die verfügbaren Aktionen werden je Bericht konfiguriert.

---

# 12. API-Anbindung

## reportApi.js

Empfohlene zentrale API-Datei:

```text
src/api/reportApi.js
```

Aufgaben:

```text
Reports laden

Filterparameter übertragen

Exportdateien anfordern

Fehler behandeln
```

---

## Hooks

Empfohlene Hooks:

```text
useReport

useReportExport
```

Diese kapseln:

- Ladezustände
- Fehlerzustände
- Aktualisierung
- Exportaufrufe

---

# 13. Rollenabhängige Anzeige

Die Sichtbarkeit von Reporting-Menüpunkten und Aktionen richtet sich nach dem bestehenden Rollenmodell.

Vorhandene Rollen:

```text
VIEWER

EDITOR

ADMIN
```

Details werden im separaten Berechtigungskonzept festgelegt.

Im Frontend gilt:

```text
Nicht erlaubte Funktionen werden nicht prominent angeboten.

Das Backend bleibt trotzdem führend für Berechtigungsprüfungen.
```

Für die erste Reportinggeneration wird ein konservativer Ansatz empfohlen.

Bis zur Definition eines eigenständigen Reporting-Berechtigungskonzepts sollen Reportingfunktionen ausschließlich für Benutzer mit der Rolle ADMIN verfügbar sein.

Dadurch können Datenschutz- und Berechtigungsanforderungen zunächst einfach und sicher umgesetzt werden.

Eine spätere Erweiterung um differenzierte Reporting-Berechtigungen bleibt ausdrücklich vorgesehen.

---

# 14. Tablet- und Laptop-Nutzung

Die Reporting-Oberfläche muss weiterhin für Tablet und Laptop nutzbar bleiben.

Wichtige Anforderungen:

```text
übersichtliche Filterbereiche

gut erreichbare Aktionsbuttons

keine überladenen Tabellen

responsive Navigation

lesbare Ergebnisdarstellung
```

Für komplexere Tabellen kann horizontales Scrollen akzeptabel sein.

---

# 15. Auswirkungen auf bestehende Frontend-Struktur

Die Einführung des Reporting-Bereichs erfordert eine allgemeine Hauptnavigation.

Die bestehende Mitgliederverwaltung wird dadurch nicht fachlich verändert.

Es entsteht jedoch eine neue strukturelle Ebene:

```text
AppLayout
├── Hauptnavigation
├── Benutzer-/Versionsinformationen
└── Inhaltsbereich
```

Die Umsetzung sollte so erfolgen, dass spätere Bereiche wie Dashboards oder weitere Fachmodule ergänzt werden können.

---

# 16. Teststrategie

Für das Frontend-Reporting werden gezielte Tests empfohlen.

Mindestumfang:

```text
Rendering der Reporting-Übersicht

Anzeige erlaubter Menüpunkte nach Rolle

Filterkomponenten

Exportbutton-Anzeige

Fehler- und Ladezustände
```

End-to-End-Tests sind nicht Bestandteil dieser Phase, können aber später über eine eigene Anforderung ergänzt werden.

---

# 17. Abgrenzung dieser Phase

Nicht Bestandteil von BL-020 Phase 1 sind:

```text
konkrete Mitgliederberichte

konkrete Geburtstagslisten

konkrete Zugehörigkeitsberichte

konkrete PDF-Layouts

konkrete Excel-Layouts

konkrete Statistikdiagramme
```

Diese Themen werden in den jeweiligen Folgephasen behandelt.

---

# 18. Zusammenfassung

Das Frontend erhält mit BL-020 einen eigenständigen Reporting-Bereich.

Kernelemente:

```text
Hauptnavigation

/reports

ReportingOverviewPage

ReportFilterPanel

ReportResultTable

ReportActions

reportApi.js

useReport
```

Die Lösung baut auf bestehenden Frontend-Konzepten auf und erweitert die Anwendung schrittweise von einer Mitgliederverwaltungsoberfläche zu einer fachlich breiteren Anwendung mit Auswertungsfunktionen.

Die bestehende Mitgliederverwaltung bleibt operativer Hauptbereich.

Reporting wird als ergänzender Auswertungsbereich eingeführt.
