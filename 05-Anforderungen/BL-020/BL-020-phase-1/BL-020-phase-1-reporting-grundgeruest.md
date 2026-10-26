# BL-020 Phase 1 – Reporting-Grundgerüst

## Status

In Arbeit

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Roadmap-Bezug

P3 Reporting und Auswertung

## Priorität

Hoch

---

# Zielsetzung

Ziel dieser Phase ist die Schaffung eines einheitlichen technischen und fachlichen Fundaments für alle zukünftigen Reportingfunktionen der EMC Mitgliederverwaltung.

Es sollen noch keine konkreten Fachberichte umgesetzt werden.

Stattdessen wird die gemeinsame Architektur definiert, auf deren Basis die nachfolgenden Reportingphasen aufgebaut werden.

---

# Ausgangssituation

Im Rahmen von BL-012 Reportingstrategie wurde entschieden, dass das zukünftige Reporting schrittweise in die Service-Welt migriert werden soll.

Die zukünftigen Berichte umfassen unter anderem:

* Mitgliederverzeichnis
* Geburtstagslisten
* Zugehörigkeitsauswertungen
* Kontrollberichte
* Mitgliederdatenblätter
* Statistische Auswertungen
* Dokumentgenerierung

Für diese Berichte existiert bislang noch keine gemeinsame technische Architektur.

---

# Ziele der Phase

## G-001 Gemeinsame Reporting-Architektur

Definition einer einheitlichen Architektur für:

* Backend
* Frontend
* Exportfunktionen

Ziel ist die Vermeidung individueller Einzellösungen je Bericht.

---

## G-002 Gemeinsame Navigationsstruktur

Definition der zukünftigen Benutzerführung.

Zu klären:

* eigener Menüpunkt „Reporting“
* Gruppierung von Berichten
* Zugriffsrechte
* Bedienkonzept

---

## G-003 Gemeinsame Backend-Struktur

Definition einer standardisierten Struktur für Reporting-Endpunkte.

Zu bewerten:

* Controller-Struktur
* Service-Struktur
* DTO-Konzept
* Filterkonzept

---

## G-004 Gemeinsame Frontend-Struktur

Definition eines standardisierten Frontend-Konzepts.

Zu bewerten:

* Reportseiten
* Filterdialoge
* Tabellenansichten
* Ergebnisdarstellung
* Wiederverwendbarkeit

---

## G-005 Exportstrategie

Definition der zukünftigen Exportarchitektur.

Zu bewerten:

* PDF
* Excel
* CSV

Zu entscheiden:

* welche Formate unterstützt werden
* welche Bibliotheken geeignet sind
* welche Formate je Bericht erforderlich sind

---

## G-006 Berechtigungskonzept

Definition der Berechtigungen für Reportingfunktionen.

Zu prüfen:

* VIEWER
* EDITOR
* ADMIN

Ziel:

ein konsistentes Rollenmodell für alle Berichte.

---

# Fachliche Fragestellungen

Im Rahmen der Analyse sollen insbesondere beantwortet werden:

* Wie werden Berichte fachlich kategorisiert?
* Welche Berichte benötigen Filter?
* Welche Berichte benötigen Exporte?
* Welche Berichte benötigen Druckfunktionen?
* Welche Berichte benötigen Parameterdialoge?
* Welche Berichte erzeugen Dokumente?

---

# Nicht Bestandteil dieser Phase

Nicht Bestandteil von Phase 1 sind:

* konkrete Mitgliederlisten
* Geburtstagslisten
* Zugehörigkeitslisten
* Ehrungsauswertungen
* Statistiken
* Dokumentgenerierung

Diese werden in den nachfolgenden Phasen behandelt.

---

# Erwartete Ergebnisse

Die Phase soll liefern:

* Zielarchitektur Reporting
* Frontend-Konzept
* Backend-Konzept
* Exportstrategie
* Rollen- und Berechtigungskonzept
* Empfehlungen für die nachfolgenden Reportingphasen

---

# Erfolgskriterien

Die Phase gilt als erfolgreich abgeschlossen, wenn:

* eine konsistente Reportingarchitektur definiert wurde
* die zukünftige Menüstruktur festgelegt wurde
* die Exportstrategie definiert wurde
* das Berechtigungskonzept festgelegt wurde
* die Grundlage für die Umsetzung der Mitgliederverzeichnisse geschaffen wurde

---

# Nachfolgephase

Nach Abschluss dieser Phase beginnt:

```text
BL-020 Phase 2 – Mitgliederverzeichnis
```
