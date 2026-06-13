# 05.160-BL_012-reportingstrategie.md

Stand: 14.06.2026

Status: Abgeschlossen

Anforderung: BL-012 Reportingstrategie

Vorgängerphasen:

- BL-012a Reporting Discovery
- BL-012b Reportingstrategie

---

# 1. Zielsetzung

Dieses Dokument beschreibt die zukünftige Reportingstrategie der EMC Mitgliederverwaltung.

Es konsolidiert die Ergebnisse aus:

- BL-012a Reporting Discovery
- BL-012b Reportingstrategie

und bildet die fachliche Referenz für die zukünftige Migration des bestehenden Access-Reportings.

Ziele:

- vollständige Ablösung des Access-Reportings
- Konsolidierung bestehender Berichte
- Dokumentation der Fachlogik
- Definition einer langfristigen Zielarchitektur
- Vorbereitung zukünftiger Auswertungen und Dashboards
- Berücksichtigung möglicher Anforderungen aus EMC Finanzen

Dieses Dokument ersetzt das bisherige Reporting-Inventar als primäre fachliche Referenz.

---

# 2. Ausgangssituation

Die bestehende Reporting-Landschaft stammt aus der Access-basierten Mitgliederverwaltung.

Zu Beginn der Analyse bestand die Befürchtung, dass das Reporting eine wesentliche Hürde für die vollständige Ablösung der Access-Anwendung darstellen könnte.

Die Discovery-Phase zeigte jedoch:

- keine komplexen Reporting-Frameworks
- keine umfangreiche VBA-Landschaft
- keine kritischen technischen Abhängigkeiten
- keine unverständlichen Spezialmechanismen

Die vorhandenen Berichte basieren überwiegend auf:

- Mitgliederlisten
- Ehrungsvorschlägen
- Ehrungshistorien
- Kontrolllisten
- einfachen statistischen Auswertungen

Die eigentliche Komplexität liegt nicht in der Berichtserzeugung, sondern in der zugrunde liegenden Fachlogik.

---

# 3. Bestandsaufnahme der Reporting-Landschaft

## 3.1 Mitgliederwesen

Vorhandene Berichte:

### rptMitgliederliste

Standard-Mitgliederverzeichnis mit Gruppierungen nach Status und Stimme.

---

### rptMitgliederlistekompakt

Kompakte Darstellung derselben Datenbasis.

---

### rptMitgliederlistefortlaufend

Fortlaufende Mitgliederliste ohne Gruppenumbrüche.

---

### rptMitgliederlisteKammerchor

Variante für Kammerchor-Mitglieder.

---

### rptMitgliederdatenblatt

Ausgabe der gespeicherten Mitgliedsdaten zur Überprüfung durch das Mitglied.

Enthält zusätzlich die Ehrungshistorie.

---

## 3.2 Geburtstagswesen

### rptGeburtstagsliste

Dient der Vorbereitung der Geburtstagsgratulation während der Chorproben.

Besonderheiten:

- Anzeige des aktuellen Alters
- Kennzeichnung von Jubiläumsgeburtstagen
- Vorbereitung von Weinpräsenten

Wichtige fachliche Regel:

Geburtstagsgratulationen sind keine Ehrungen.

Es erfolgt keine Speicherung in der Ehrungshistorie.

---

## 3.3 Ehrungswesen

### rptZugehoerigkeit

Ermittlung möglicher Ehrungskandidaten auf Basis der Vereinszugehörigkeit.

---

### rptZugehoerigkeitnachJahrensortiert

Alternative Darstellung der Ehrungsvorschläge.

---

### rptEhrungenvonMitgliedern

Historie bereits vergebener Ehrungen.

---

### rptEhrungenvonEhrenmitgliedern

Spezialvariante für Ehrenmitglieder.

---

### Fachliche Grundlagen

Vorhanden sind:

#### tblEhrungen_FT

Katalog aller möglichen Ehrungsarten.

Beispiele:

- EMC Jubiläen
- DCV Jubiläen
- Ehrenmitglied
- Ehrenpräsident

#### tblEhrungen

Historie tatsächlich durchgeführter Ehrungen.

---

## 3.4 Kontrollberichte

### rptDatenschutz

Kontrolle datenschutzrelevanter Informationen.

---

### rptVerteiler

Kontrolle der E-Mail-Verteiler.

---

### rptChorkleidung

Kontrolle ausgegebener Chorkleidung.

---

## 3.5 Statistik

### rptAltersdurchschnitt

Einfache statistische Auswertung.

---

### rptAltersstruktur

Historisch gewachsener Statistikbericht.

Technischer Aufbau:

Query
↓
Makro
↓
VBA
↓
Hilfstabelle
↓
weitere Query
↓
Diagrammbericht

Zusätzlich ist ein manueller Aktualisierungsschritt erforderlich.

Bewertung:

Technische Altlast.

Die technische Umsetzung wird nicht übernommen.

# 4. Wesentliche Erkenntnisse der Discovery

## 4.1 Reporting-Landschaft kleiner als erwartet

Die Anzahl der vorhandenen Access-Berichte suggeriert zunächst eine größere Komplexität.

Die Analyse zeigte jedoch:

Viele Berichte stellen lediglich unterschiedliche Darstellungsvarianten derselben fachlichen Auswertung dar.

Beispiel:

```text
Mitgliederliste
Mitgliederliste kompakt
Mitgliederliste fortlaufend
Mitgliederliste Kammerchor
```

Dadurch reduziert sich die tatsächliche fachliche Komplexität erheblich.

---

## 4.2 Fachlogik wichtiger als Berichtstechnik

Die größte Herausforderung liegt nicht in der technischen Berichtserzeugung.

Fachlich relevant sind insbesondere:

- Geburtstagswesen
- EMC-Ehrungen
- DCV-Ehrungen
- Zugehörigkeiten
- Ehrungshistorie

Diese Logiken sind aktuell auf Queries und Berichte verteilt.

---

## 4.3 Exportfunktionen sind überschaubar

Identifiziert wurden:

- Excel-Exporte
- CSV-Exporte
- einzelne Standardexporte

Die Exportlandschaft stellt kein wesentliches Migrationsrisiko dar.

---

## 4.4 Dokumentgenerierung ist überschaubar

Vorhanden ist im Wesentlichen:

### Nutzungsvereinbarung Chorkleidung

Merkmale:

- Einzel-Dokument
- keine Serienbriefverarbeitung
- Word-Vorlage
- Bookmark-Befüllung

Bewertung:

Technisch überschaubar.

---

## 4.5 VBA-Einsatz minimal

Relevanter VBA-Einsatz wurde ausschließlich im Zusammenhang mit der Altersstruktur gefunden.

Es wurde keine kritische Vereinslogik innerhalb des VBA-Codes identifiziert.

---

# 5. Konsolidierungspotential

## 5.1 Mitgliederlistenfamilie

Heute:

```text
rptMitgliederliste
rptMitgliederlistekompakt
rptMitgliederlistefortlaufend
rptMitgliederlisteKammerchor
```

Zukünftig:

```text
Mitgliederverzeichnis
```

mit Parametern:

- Standard
- Kompakt
- Fortlaufend
- Kammerchor

Die Unterschiede werden ausschließlich durch Filter und Layoutvarianten abgebildet.

---

## 5.2 Ehrungswesen

Heute:

```text
rptZugehoerigkeit
rptZugehoerigkeitnachJahrensortiert
rptEhrungenvonMitgliedern
rptEhrungenvonEhrenmitgliedern
```

Zukünftig:

```text
Ehrungsmodul
```

mit:

- Ehrungsvorschlägen
- Ehrungshistorie
- Ehrungsarten
- Filterung
- PDF-Ausgaben

---

## 5.3 Kontrollberichte

Heute:

- Datenschutz
- Verteiler
- Chorkleidung

Zukünftig:

Filterbare Listenansichten mit:

- PDF-Export
- Excel-Export
- CSV-Export

Separate Berichte sind hierfür häufig nicht mehr notwendig.

---

## 5.4 Statistik

Heute:

- Altersdurchschnitt
- Altersstruktur

Zukünftig:

```text
Statistikmodul
```

mit:

- Kennzahlen
- Diagrammen
- Trendanalysen

Die technische Umsetzung der Access-Altersstruktur wird nicht übernommen.

---

# 6. Zielbild Reporting

## Grundprinzip

Reporting wird künftig nicht als Sammlung einzelner Berichte verstanden.

Stattdessen entsteht eine gemeinsame Auswertungsplattform.

```text
Fachlogik
      ↓
Reporting-Service
      ↓
Ausgabeformat
      ├── Bildschirm
      ├── PDF
      ├── Excel
      ├── CSV
      ├── Word
      └── Dashboard
```

Die Fachlogik wird einmal implementiert und anschließend für mehrere Ausgabeformate genutzt.

---

## Arten von Ausgaben

### PDF-Berichte

Verwendung:

- Vorstandsunterlagen
- Ehrungsvorschläge
- Geburtstagslisten
- Mitgliederdatenblätter
- Archivierung

PDF wird langfristig das primäre Druckformat.

---

### Excel-Exporte

Verwendung:

- externe Weiterverarbeitung
- Sonderauswertungen
- Serienbriefvorbereitung

Excel bleibt ein wichtiger Bestandteil der Reportingstrategie.

---

### CSV-Exporte

Verwendung:

- Datenaustausch
- Import in Fremdsysteme
- individuelle Auswertungen

---

### Word-Dokumente

Verwendung:

- Nutzungsvereinbarung Chorkleidung
- spätere Bescheinigungen
- individualisierte Dokumente

---

### Browser-Druckansichten

Verwendung:

- schnelle Ausdrucke
- Ad-hoc-Auswertungen

---

### Dashboards

Verwendung:

- Kennzahlen
- Vorstandsübersichten
- Trends

Nicht Bestandteil der ersten Migrationsphasen.

# 7. Zielarchitektur

## Architekturprinzipien

Die zukünftige Reportingarchitektur orientiert sich an folgenden Grundsätzen:

### Kein Big-Bang

Berichte werden einzeln migriert.

Jeder Bericht wird vor der Umsetzung fachlich und technisch bewertet.

---

### Fachlogik zentralisieren

Fachliche Regeln werden nicht mehr in Berichten versteckt.

Sie werden zentral im Backend implementiert.

Beispiele:

- Geburtstagsregeln
- Ehrungsregeln
- Jubiläumslogik
- Zugehörigkeitsberechnungen

---

### Darstellung von Fachlogik trennen

Die Berechnung einer Auswertung wird von ihrer Darstellung getrennt.

Dadurch kann dieselbe Auswertung mehrfach verwendet werden.

Beispiel:

```text id="4y0h7m"
Geburtstagsauswertung
        ↓
PDF

Geburtstagsauswertung
        ↓
Excel

Geburtstagsauswertung
        ↓
Dashboard
```

---

## Architekturvarianten

### Option A – Fest programmierte Berichte

#### Beschreibung

Jeder Bericht wird individuell implementiert.

```text id="n9rjlwm"
Bericht
↓
eigene Logik
↓
eigene Ausgabe
```

#### Vorteile

- einfacher Einstieg
- geringe Anfangskomplexität

#### Nachteile

- hohe Wartungskosten
- Logik wird mehrfach implementiert
- geringe Wiederverwendbarkeit

#### Bewertung

Für einzelne Berichte geeignet.

Nicht als langfristige Gesamtstrategie.

---

### Option B – Template-basierte Berichte

#### Beschreibung

Fachlogik und Layout werden getrennt.

```text id="3q4nmx"
Daten
+
Template
=
Bericht
```

#### Vorteile

- hohe Wartbarkeit
- Layoutänderungen einfacher
- gute Wiederverwendung

#### Nachteile

- Template-Verwaltung erforderlich

#### Bewertung

Sehr gut geeignet für EMC Mitglieder.

---

### Option C – Generische Reportingplattform

#### Beschreibung

Einführung einer vollständigen Reporting-Lösung.

Beispiele:

- JasperReports
- ähnliche Frameworks

#### Vorteile

- hohe Flexibilität
- professionelle Reporting-Funktionen

#### Nachteile

- zusätzliche Technologie
- hoher Schulungsaufwand
- Overengineering für den aktuellen Umfang

#### Bewertung

Derzeit nicht empfohlen.

---

### Option D – Hybrider Ansatz

#### Beschreibung

Eigene Reporting-Services kombiniert mit Templates und Exportmodulen.

```text id="c7wl3f"
Reporting API
      ↓
Fachmodule
      ↓
Templates
      ↓
PDF / Excel / CSV / Word
```

#### Vorteile

- pragmatisch
- wartbar
- ausbaufähig
- EMC-Finanzen-kompatibel

#### Nachteile

- etwas höherer Initialaufwand

#### Bewertung

Empfohlene Zielarchitektur.

---

## Zielarchitektur für EMC Mitglieder

### Backend

Das Spring-Boot-Backend stellt Reporting-Services bereit.

Beispiele:

```text id="cc6o6j"
MemberReportService
BirthdayReportService
HonorReportService
StatisticsReportService
```

Diese Services liefern fachlich aufbereitete Daten.

---

### Reporting-Schicht

Die Reporting-Schicht übernimmt:

- Filterung
- Sortierung
- Gruppierung
- Datenaufbereitung

Nicht jedoch die eigentliche Darstellung.

---

### Ausgabeebene

Die Ausgabe erfolgt über:

```text id="hn0lfi"
PDF
Excel
CSV
Word
Browseransicht
```

Alle Formate greifen auf dieselbe fachliche Datenbasis zu.

---

### Frontend

Das React-Frontend dient als Bedienoberfläche.

Funktionen:

- Bericht auswählen
- Parameter setzen
- Export starten
- Download bereitstellen

---

# 8. Dokumentgenerierung

## PDF

### Rolle

PDF wird das primäre Dokument- und Druckformat.

### Einsatzgebiete

- Mitgliederlisten
- Geburtstagslisten
- Ehrungsvorschläge
- Mitgliederdatenblätter

### Priorität

Sehr hoch.

---

## Excel

### Rolle

Wichtigster Exportweg für Weiterverarbeitung.

### Einsatzgebiete

- Sonderauswertungen
- Datenanalysen
- Serienbriefvorbereitung

### Priorität

Hoch.

---

## CSV

### Rolle

Standardformat für Datenaustausch.

### Einsatzgebiete

- Fremdsysteme
- individuelle Auswertungen

### Priorität

Hoch.

---

## Word

### Rolle

Spezialformat für editierbare Dokumente.

### Einsatzgebiete

- Nutzungsvereinbarung Chorkleidung
- Bescheinigungen
- spätere individualisierte Dokumente

### Priorität

Mittel.

---

## Browser-Druckansicht

### Rolle

Komfortfunktion.

### Einsatzgebiete

- schnelle Ausdrucke
- spontane Auswertungen

### Priorität

Niedrig.

---

# 9. Dashboards und Statistik

## Grundsatz

Dashboards sind kein Bestandteil der Access-Ablösung.

Die Architektur soll jedoch so gestaltet werden, dass sie später ergänzt werden können.

---

## Sinnvolle Kennzahlen

### Mitgliederbestand

- aktive Mitglieder
- passive Mitglieder
- Ehrenmitglieder

---

### Stimmenverteilung

- Sopran
- Alt
- Tenor
- Bass

---

### Altersstatistik

- Durchschnittsalter
- Altersgruppen
- Altersverteilung

---

### Mitgliedsentwicklung

- Eintritte
- Austritte
- Saldo

---

### Ehrungswesen

- anstehende Ehrungen
- Ehrungen pro Jahr
- Ehrungsarten

---

## Sinnvolle Diagramme

- Altersverteilung
- Stimmenverteilung
- Mitgliederentwicklung
- Ehrungsentwicklung

---

## Architekturvorbereitung

Die Reporting-Services sollen Daten unabhängig vom Ausgabeformat liefern.

Dadurch können zukünftige Dashboards dieselben Datenquellen nutzen wie PDF- oder Excel-Berichte.

# 10. Rollen und Berechtigungen

## Grundprinzip

Das Reporting soll sich in das bestehende Rollen- und Berechtigungskonzept der EMC Mitgliederverwaltung integrieren.

Nicht jeder Benutzer benötigt Zugriff auf alle Auswertungen oder Exportfunktionen.

Die Berechtigungen sollen fachlich nachvollziehbar und einfach administrierbar bleiben.

---

## Normale Benutzer

Dürfen:

- Standardlisten anzeigen
- eigene Such- und Filterfunktionen nutzen
- freigegebene Standardberichte erzeugen

Beispiele:

- Mitgliederverzeichnis
- allgemeine Listenansichten

---

## Vorstand

Dürfen zusätzlich:

- Geburtstagslisten erzeugen
- Ehrungsvorschläge erzeugen
- Ehrungshistorien einsehen
- vereinsbezogene Auswertungen durchführen

Beispiele:

- Geburtstagswesen
- Ehrungswesen
- Jubiläumsvorschläge

---

## Administratoren

Dürfen zusätzlich:

- alle Berichte erzeugen
- Exporte durchführen
- Dokumentgenerierung nutzen
- Reporting-Konfiguration verwalten

Beispiele:

- Excel-Exporte
- CSV-Exporte
- Dokumentvorlagen

---

## Langfristige Erweiterungsmöglichkeiten

Bei Bedarf können später zusätzliche Berechtigungsstufen eingeführt werden.

Beispiele:

- Ehrungsbeauftragter
- Datenschutzbeauftragter
- Chorleitung
- Stimmführer

Für die erste Reportinggeneration wird dies jedoch nicht benötigt.

---

# 11. EMC-Finanzen-Kompatibilität

## Ausgangslage

Die Anforderungen von EMC Mitglieder und EMC Finanzen unterscheiden sich erheblich.

EMC Mitglieder benötigt überwiegend:

- Listen
- Ehrungsvorschläge
- Kontrollauswertungen
- einfache Statistiken

EMC Finanzen wird voraussichtlich benötigen:

- Summierungen
- Gruppierungen
- Jahresauswertungen
- Geschäftsberichte
- Steuerrelevante Auswertungen
- Finanzdiagramme

---

## Für EMC Mitglieder ausreichend

Folgende Ansätze wären ausschließlich für EMC Mitglieder ausreichend:

### Einzelberichte

Fest programmierte Einzelberichte.

### Direkte PDF-Ausgaben

Bericht erzeugt direkt PDF.

### Einfache Listenexporte

CSV- und Excel-Exporte ohne gemeinsame Architektur.

---

## Für EMC Finanzen ungeeignet

Folgende Ansätze würden langfristig an Grenzen stoßen:

- Berichtslogik direkt in PDF-Ausgaben
- voneinander unabhängige Einzelberichte
- mehrfach implementierte Fachlogik

---

## Für beide Produkte geeignet

### Gemeinsame Reporting-API

```text id="mmp9n0"
Fachlogik
      ↓
Reporting Service
      ↓
Ausgabeformat
```

---

### Template-basierte Ausgabe

```text id="f4j2zo"
Daten
+
Template
=
Bericht
```

---

### Standardisierte Exportformate

```text id="vy6xrb"
PDF
Excel
CSV
```

---

## Strategische Bewertung

Die empfohlene hybride Reportingarchitektur erfüllt bereits heute die Anforderungen von EMC Mitglieder.

Gleichzeitig schafft sie die Grundlage für spätere Finanzberichte, ohne heute unnötige Komplexität einzuführen.

---

# 12. Migrationsstrategie

## Grundsatz

Die Migration erfolgt schrittweise.

Jeder Bericht wird einzeln bewertet und umgesetzt.

Ein Big-Bang-Ansatz wird ausdrücklich ausgeschlossen.

---

## Phase 1 – Reporting-Grundgerüst

Ziele:

- Reporting-API definieren
- Berichtstypen definieren
- Exportarchitektur festlegen
- Berechtigungen integrieren

Ergebnis:

Technische Grundlage für alle weiteren Schritte.

---

## Phase 2 – Mitgliederverzeichnis

Migration:

```text id="dcn0nb"
rptMitgliederliste
rptMitgliederlistekompakt
rptMitgliederlistefortlaufend
rptMitgliederlisteKammerchor
```

Ziel:

Gemeinsamer Berichtstyp:

```text id="7vhjl0"
Mitgliederverzeichnis
```

Nutzen:

- hohe Sichtbarkeit
- geringe Komplexität
- schneller Erfolg

---

## Phase 3 – Geburtstagswesen

Migration:

```text id="tq4zrv"
rptGeburtstagsliste
```

Ziel:

Digitale Geburtstagsauswertung mit PDF- und Exportfunktion.

---

## Phase 4 – Kontrollberichte

Migration:

```text id="25gkzv"
rptDatenschutz
rptVerteiler
rptChorkleidung
```

Ziel:

Filterbare Listenansichten mit Exportfunktion.

---

## Phase 5 – Mitgliederdatenblatt

Migration:

```text id="6r4zyr"
rptMitgliederdatenblatt
```

Ziel:

PDF-Dokument mit integrierter Ehrungshistorie.

---

## Phase 6 – Ehrungswesen

Migration:

```text id="u6w0yo"
rptZugehoerigkeit
rptZugehoerigkeitnachJahrensortiert
rptEhrungenvonMitgliedern
rptEhrungenvonEhrenmitgliedern
```

Ziel:

Gemeinsames Ehrungsmodul.

Beinhaltet:

- Ehrungsvorschläge
- Ehrungshistorie
- EMC-Ehrungen
- DCV-Ehrungen

---

## Phase 7 – Statistik

Migration:

```text id="n9g4vi"
rptAltersdurchschnitt
rptAltersstruktur
```

Wichtig:

Die bestehende technische Umsetzung der Altersstruktur wird nicht übernommen.

Es erfolgt eine vollständige Neuimplementierung auf Basis moderner Auswertungsmechanismen.

---

## Phase 8 – Dokumentgenerierung

Migration:

```text id="4d1lmr"
Nutzungsvereinbarung Chorkleidung
```

Ziel:

Zentrale Vorlagenverwaltung ohne lokale Pfadabhängigkeiten.

---

# 13. Getroffene Architekturentscheidungen

## Entscheidung 1 – Hybrides Reportingmodell

Die EMC Mitgliederverwaltung verfolgt künftig ein hybrides Reportingmodell.

Architektur:

```text id="8p0h55"
Reporting Services
+
Templates
+
PDF / Excel / CSV
```

Eine generische Reportingplattform wird zunächst nicht eingeführt.

Begründung:

Die bestehende Reporting-Landschaft ist fachlich überschaubar und rechtfertigt derzeit keine zusätzliche Reportingplattform.

---

## Entscheidung 2 – Konsolidierung der Mitgliederlisten

Die bestehenden Berichte:

- Mitgliederliste
- Mitgliederliste kompakt
- Mitgliederliste fortlaufend
- Mitgliederliste Kammerchor

werden künftig als Varianten eines gemeinsamen Berichtstyps betrachtet.

Bezeichnung:

```text id="qt8aez"
Mitgliederverzeichnis
```

Unterschiede werden über Filter- und Layoutparameter abgebildet.

---

## Entscheidung 3 – Fachlogik zentralisieren

Fachliche Regeln werden künftig ausschließlich in Backend-Services implementiert.

Berichte dienen ausschließlich der Darstellung.

---

## Entscheidung 4 – Altersstruktur neu konzipieren

Die bestehende Access-Lösung wird nicht migriert.

Die fachliche Auswertung bleibt erhalten.

Die technische Umsetzung wird vollständig neu entwickelt.

---

# 14. Roadmap

## Kurzfristig

- Reportingstrategie verabschieden
- Entscheidungen dokumentieren
- Reporting-Backlog ableiten

---

## Mittelfristig

- Reporting-Grundarchitektur definieren
- Mitgliederverzeichnis migrieren
- Geburtstagswesen migrieren
- Kontrollberichte migrieren

---

## Langfristig

- Ehrungswesen migrieren
- Statistikmodul aufbauen
- Dashboards ergänzen
- EMC-Finanzen integrieren

---

# 15. Abschlussbewertung

Die Analyse der bestehenden Reporting-Landschaft hat gezeigt, dass die vollständige Ablösung des Access-Reportings realistisch ist.

Die größte Herausforderung liegt nicht in der Berichtserzeugung selbst, sondern in:

- der Dokumentation der Fachlogik
- der Konsolidierung historisch gewachsener Berichte
- der Vereinheitlichung von Auswertungen
- der Definition einer langfristig tragfähigen Architektur

Die zukünftige Reportinglandschaft basiert auf einem hybriden Ansatz aus:

- zentralen Reporting-Services
- templatebasierter Ausgabe
- standardisierten Exportformaten

Dadurch werden gleichzeitig erreicht:

- vollständige Access-Ablösung
- einfache Erweiterbarkeit
- Wiederverwendbarkeit für EMC Finanzen
- Vorbereitung zukünftiger Dashboards
- geringe technische Komplexität

---

# Abschlussfazit

Status:

```text id="udj6kg"
BL-012 Reportingstrategie
ABGESCHLOSSEN
```

Die Reporting-Landschaft der EMC Mitgliederverwaltung ist analysiert, konsolidiert und strategisch geplant.

Eine belastbare Grundlage für die zukünftige schrittweise Ablösung des Access-Reportings ist vorhanden.
