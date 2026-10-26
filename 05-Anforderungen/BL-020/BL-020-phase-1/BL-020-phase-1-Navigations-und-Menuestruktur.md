# BL-020 Phase 1 – Navigations- und Menüstruktur

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 1 – Reporting-Grundgerüst

---

# 1. Zielsetzung

Dieses Dokument definiert die zukünftige Navigations- und Menüstruktur für Reportingfunktionen der EMC Mitgliederverwaltung.

Ziel ist die Schaffung einer konsistenten Benutzerführung für zukünftige Auswertungen und Berichte.

Die Struktur soll:

* einfach verständlich sein
* mit der bestehenden Anwendung harmonieren
* zukünftige Erweiterungen ermöglichen
* eine schrittweise Migration des Access-Reportings unterstützen

---

# 2. Ausgangssituation

Die aktuelle Anwendung konzentriert sich auf:

* Mitgliederverwaltung
* Mitgliedersuche
* Mitgliedspflege
* Administration

Ein eigenständiger Reporting-Bereich existiert bislang nicht.

Im Rahmen von BL-020 soll erstmals eine gemeinsame Benutzeroberfläche für Auswertungen geschaffen werden.

---

# 3. Grundprinzipien

## Trennung von Fachfunktionen und Reporting

Reporting dient der Auswertung von Datenbeständen.

Fachliche Arbeitsprozesse verbleiben in den jeweiligen Fachmodulen.

Beispiele:

```text
Mitglied bearbeiten
→ Fachfunktion

Chorkleidung pflegen
→ Fachfunktion

Benutzer verwalten
→ Fachfunktion

Geburtstagsliste erzeugen
→ Reporting

Mitgliederverzeichnis erzeugen
→ Reporting
```

---

## Eigenständiger Reporting-Bereich

Reporting erhält einen eigenen Hauptnavigationspunkt.

Begründung:

* bessere Auffindbarkeit
* klare fachliche Zuordnung
* Erweiterbarkeit
* Vorbereitung zukünftiger Statistik- und Dashboardfunktionen

---

## Wiederverwendbare Bedienkonzepte

Berichte sollen künftig möglichst dieselben Bedienmuster verwenden.

Insbesondere:

* Filterdialoge
* Ergebnisansichten
* Exportfunktionen
* Druckfunktionen

sollen einheitlich gestaltet werden.

---

# 4. Hauptnavigation

Für die erste Reportinggeneration wird folgende Hauptnavigation empfohlen:

```text
Mitglieder

Reporting

Administration
```

---

## Mitglieder

Enthält:

```text
Mitgliederliste

Mitgliedspflege

Mitgliedsdetails
```

Zweck:

Operative Arbeit mit Mitgliederdaten.

---

## Reporting

Enthält:

```text
Mitglieder

Geburtstage

Zugehörigkeiten

Kontrollberichte

Statistik
```

Zweck:

Auswertungen und Berichte.

---

## Administration

Enthält:

```text
Benutzerverwaltung

Systeminformationen

zukünftige Administrationsfunktionen
```

---

# 5. Auswirkungen auf die bestehende Frontend-Struktur

Die Einführung eines eigenständigen Reporting-Bereichs erweitert die bisherige Navigationsstruktur der EMC Mitgliederverwaltung.

Bislang liegt der fachliche Schwerpunkt auf:

```text
Mitgliederverwaltung
```

Mit Einführung des Reportings entsteht erstmals eine dauerhafte Hauptnavigation mit mehreren gleichberechtigten Fachbereichen.

Zielbild:

```text
Mitglieder

Reporting

Administration
```

---

## Grundsatz

Die bestehenden Fachfunktionen werden durch BL-020 fachlich nicht verändert.

Insbesondere bleiben erhalten:

```text
Mitgliederliste

Mitgliedersuche

Mitgliederdetailansicht

Mitgliedspflege

Administration
```

BL-020 ergänzt die Anwendung lediglich um einen zusätzlichen Fachbereich:

```text
Reporting
```

---

## Auswirkungen auf die Benutzerführung

Die Mitgliederverwaltung bleibt der primäre Arbeitsbereich der Anwendung.

Reporting dient als ergänzender Auswertungsbereich.

Die Benutzerführung soll daher weiterhin auf den operativen Arbeitsablauf ausgerichtet bleiben:

```text
Mitglieder verwalten
↓
Mitglied pflegen
↓
Mitglied suchen
```

und nicht durch Reporting dominiert werden.

---

## Auswirkungen auf das Frontend-Layout

Für die Integration des Reporting-Bereichs wird eine allgemeine Hauptnavigation erforderlich.

Die konkrete technische Umsetzung wird in der späteren Frontend-Architektur definiert.

Mögliche Varianten:

```text
Top-Navigation

Side-Navigation

Responsive Kombination
```

Die Lösung muss sowohl für Laptop- als auch Tablet-Nutzung geeignet sein.

---

## Auswirkungen auf zukünftige Erweiterungen

Die Einführung der Hauptnavigation schafft gleichzeitig die Grundlage für spätere Erweiterungen.

Beispiele:

```text
Reporting

Dashboards

Dokumente

weitere Fachmodule
```

Dadurch wird die Anwendung langfristig von einer reinen Mitgliederverwaltung zu einer vollständigen Fachanwendung weiterentwickelt.

---

# 6. Reporting-Menüstruktur

## Mitglieder

Berichte:

```text
Mitgliederverzeichnis

Mitgliederdatenblatt
```

---

### Mitgliederverzeichnis

Konsolidiert künftig:

```text
Mitgliederliste

Mitgliederliste kompakt

Mitgliederliste fortlaufend

Kammerchor-Liste
```

Unterschiede werden über Parameter und Darstellungsvarianten gesteuert.

---

### Mitgliederdatenblatt

Erzeugung eines Datenblatts für:

* einzelnes Mitglied
* mehrere Mitglieder
* gefilterte Mitgliedermengen

Die Auswahl erfolgt über dieselben Selektionsmechanismen wie andere Mitgliederberichte.

---

## Geburtstage

Berichte:

```text
Geburtstagsliste
```

Spätere Erweiterungen:

```text
Jubiläumsgeburtstage

Geburtstagsauswertungen
```

---

## Zugehörigkeiten

Berichte:

```text
Zugehörigkeitsauswertung
```

Spätere Erweiterungen:

```text
Jubiläumsjahre

Kandidatenlisten
```

---

## Kontrollberichte

Berichte:

```text
Datenschutz

Verteiler

Chorkleidung
```

Zielgruppe:

Vorstand und Administration.

---

## Statistik

Berichte:

```text
Mitgliederbestand

Altersstruktur

Stimmenverteilung
```

Dieser Bereich wird erst in späteren Phasen umgesetzt.

---

# 7. Nicht Bestandteil des Reporting-Menüs

## Nutzungsvereinbarung Chorkleidung

Die Nutzungsvereinbarung wird nicht als Reportingfunktion betrachtet.

Begründung:

Die Erzeugung erfolgt im Zusammenhang mit einem einzelnen Mitglied und einem konkreten Fachprozess.

Ablauf:

```text
Mitglied
↓
Chorkleidung
↓
Nutzungsvereinbarung erzeugen
```

Die Funktion verbleibt daher innerhalb der Chorkleidungsverwaltung.

---

# 8. Navigationsprinzip innerhalb eines Berichts

Jeder Bericht soll künftig einem gemeinsamen Bedienmodell folgen.

Grundschema:

```text
Bericht auswählen
        ↓
Filter setzen
        ↓
Vorschau anzeigen
        ↓
Exportieren
```

---

## Filterbereich

Typische Elemente:

```text
Suche

Status

Stimme

Zeitraum

Berichtsspezifische Parameter
```

---

## Ergebnisbereich

Darstellung als:

```text
Tabelle

Gruppierte Tabelle

Kartenansicht

später Diagramme
```

abhängig vom jeweiligen Bericht.

---

## Aktionen

Mögliche Aktionen:

```text
PDF

Excel

CSV

Drucken
```

Nicht jeder Bericht muss alle Aktionen unterstützen.

---

# 9. Wiederverwendung bestehender Filterkonzepte

Die bereits vorhandenen Such- und Filtermechanismen der Mitgliederliste bilden die Grundlage zukünftiger Reportingfilter.

Vorhandene Konzepte:

```text
Freitextsuche

Statusfilter

Stimmenfilter
```

Diese sollen möglichst wiederverwendet werden.

Ziel:

Vermeidung mehrfach implementierter Filterlogik.

---

# 10. Erweiterbarkeit

Die Menüstruktur soll zukünftige Erweiterungen unterstützen.

Mögliche spätere Bereiche:

```text
Dashboards

Finanzauswertungen

Ehrungswesen

Dokumentvorlagen
```

Die Einführung zusätzlicher Bereiche soll ohne grundlegende Änderungen der Navigationsstruktur möglich sein.

---

# 11. Zusammenfassung

Für die erste Reportinggeneration wird folgende Zielstruktur festgelegt:

```text
Mitglieder

Reporting
├── Mitglieder
│   ├── Mitgliederverzeichnis
│   └── Mitgliederdatenblatt
│
├── Geburtstage
│   └── Geburtstagsliste
│
├── Zugehörigkeiten
│   └── Zugehörigkeitsauswertung
│
├── Kontrollberichte
│   ├── Datenschutz
│   ├── Verteiler
│   └── Chorkleidung
│
└── Statistik

Administration
```

Die Struktur bildet die Grundlage für:

* Frontend-Konzept
* Routing-Konzept
* Berechtigungskonzept
* Reporting-Architektur

und dient als Referenz für die nachfolgenden Reportingphasen.
