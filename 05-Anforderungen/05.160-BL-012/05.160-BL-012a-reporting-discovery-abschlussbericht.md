# emc_bl_012a_reporting_discovery_abschlussbericht.md

Projekt: EMC Mitgliederverwaltung

Phase: BL-012a Reporting Discovery und Machbarkeitsanalyse

Status: Abgeschlossen

Datum: 2026-06-14

---

# 1. Zielsetzung

Ziel der Discovery-Phase BL-012a war die vollständige fachliche und technische Analyse des bestehenden Reporting-Bereichs der Access-Mitgliederverwaltung.

Im Fokus standen insbesondere:

- Inventarisierung aller Berichte
- Analyse der zugrunde liegenden Datenquellen
- Identifikation fachlicher Vereinslogik
- Bewertung der technischen Komplexität
- Untersuchung von Export- und Dokumentgenerierungsfunktionen
- Einschätzung der Migrationsfähigkeit
- Vorbereitung einer späteren Reportingstrategie (BL-012b)

Während der Discovery wurden bewusst keine Architekturentscheidungen getroffen und keine technische Umsetzung vorbereitet.

---

# 2. Durchgeführte Analyse

Analysiert wurden:

## Berichte

- rptMitgliederliste
- rptMitgliederlistekompakt
- rptMitgliederlistefortlaufend
- rptMitgliederlisteKammerchor
- rptGeburtstagsliste
- rptZugehoerigkeit
- rptZugehoerigkeitnachJahrensortiert
- rptEhrungenvonMitgliedern
- rptEhrungenvonEhrenmitgliedern
- rptMitgliederdatenblatt
- rptVerteiler
- rptDatenschutz
- rptChorkleidung
- rptAltersdurchschnitt
- rptAltersstruktur

## Datenquellen

- Tabellen
- Fachtabellen
- Queries
- Queryketten

## Technische Artefakte

- Objektabhängigkeiten
- gespeicherte Exporte
- Makros
- VBA-Module
- Word-Dokumenterzeugung

---

# 3. Wesentliche Erkenntnisse

## 3.1 Reporting-Landschaft kleiner als erwartet

Die Anzahl der vorhandenen Reports suggeriert zunächst eine komplexe Reporting-Landschaft.

Die Analyse zeigte jedoch:

Viele Reports sind lediglich Darstellungsvarianten derselben fachlichen Auswertung.

Beispiel:

```text
Mitgliederliste
Mitgliederliste kompakt
Mitgliederliste fortlaufend
Mitgliederliste Kammerchor
```

sind fachlich Varianten eines gemeinsamen Mitgliederverzeichnisses.

Dadurch reduziert sich die tatsächliche fachliche Komplexität erheblich.

---

## 3.2 Fachlogik wichtiger als Berichtstechnik

Die eigentliche Komplexität liegt nicht in der Berichtserzeugung.

Die fachlich relevanten Bereiche sind:

- Geburtstagswesen
- Ehrungswesen EMC
- Ehrungswesen DCV
- Zugehörigkeiten
- Ehrungshistorie

Diese Logiken sind aktuell überwiegend in Queries und Berichten verteilt.

---

## 3.3 Geburtstagswesen und Ehrungswesen sind getrennte Prozesse

Eine wichtige Discovery-Erkenntnis war die Trennung zwischen:

### Geburtstagsgratulation

- Gratulation während der Chorprobe
- Weinpräsente bei Jubiläumsgeburtstagen
- keine Speicherung
- keine Verbindung zu tblEhrungen

und

### Ehrungswesen

- EMC-Mitgliedsjubiläen
- DCV-Mitgliedsjubiläen
- Ehrenmitglied
- Ehrenpräsident

mit Speicherung in:

```text
tblEhrungen
```

---

## 3.4 Ehrungsmodell ist sauber strukturiert

Vorhanden sind:

### tblEhrungen_FT

Katalog möglicher Ehrungsarten.

### tblEhrungen

Historie tatsächlich durchgeführter Ehrungen.

Die Berichte rptZugehoerigkeit und rptZugehoerigkeitnachJahrensortiert dienen fachlich als Ehrungsvorschlagsberichte.

---

## 3.5 Exportfunktionen sind überschaubar

Identifiziert wurde ein relevanter Export:

```text
EMCMitgliederliste_ExportExcel
```

Datenquelle:

```text
qryMitgliederliste_ExportExcel
```

Ziel:

```text
Mitgliederliste_ExportExcel.xlsx
```

Darüber hinaus werden gelegentlich Access-Standardexporte verwendet.

Die Exportlandschaft stellt kein wesentliches Migrationsrisiko dar.

---

## 3.6 Dokumentgenerierung ist überschaubar

Vorhanden ist eine dokumentierte Word-Automation:

### Nutzungsvereinbarung Chorkleidung

Merkmale:

- Einzel-Dokument
- keine Serienbriefverarbeitung
- Word-Vorlage
- Bookmark-Befüllung
- lokaler Dateipfad

Bewertung:

Technisch überschaubar.

Keine kritische Abhängigkeit.

---

## 3.7 VBA-Einsatz minimal

Reportrelevante VBA-Logik wurde ausschließlich im Zusammenhang mit:

```text
rptAltersstruktur
```

gefunden.

Dabei wird:

```text
qryAltersstruktur_Chor
```

mittels VBA transponiert und in:

```text
tblAltersstruktur_Chor_Transponiert
```

geschrieben.

Diese Konstruktion dient ausschließlich der technischen Datenaufbereitung.

Es wurde keine kritische Vereinslogik innerhalb des VBA-Codes identifiziert.

---

# 4. Technische Altlasten

## rptAltersstruktur

Der Bericht stellt die größte technische Altlast des Reporting-Bereichs dar.

Datenfluss:

```text
Querykette
↓
Makro
↓
VBA-Transponierung
↓
Hilfstabelle
↓
weitere Querykette
↓
Diagrammbericht
```

Zusätzlich ist ein manueller Aktualisierungsschritt erforderlich.

Bewertung:

- fachlich einfach
- technisch unnötig komplex

Empfehlung:

Keine Migration der technischen Umsetzung.

Vollständige Neuimplementierung der fachlichen Auswertung.

---

## Word-Vorlage Nutzungsvereinbarung

Aktuell besteht eine feste Pfadabhängigkeit:

```text
H:\...
```

Bewertung:

Klassische Single-User-Lösung.

Empfehlung:

Zentrale Vorlagenablage auf dem NAS.

---

# 5. Migrationsfähigkeit

## Sehr gut migrierbar

- Mitgliederverzeichnis
- Geburtstagsliste
- Ehrungshistorie
- Datenschutz
- Chorkleidung
- Verteiler
- Mitgliederdatenblatt
- Altersdurchschnitt
- Exportfunktion

## Gut migrierbar

- Zugehörigkeit
- Zugehörigkeit nach Jahren sortiert

## Neu zu konzipieren

- Altersstruktur

---

# 6. Bewertung der ursprünglichen Fragestellung

Zu Beginn der Discovery bestand die Befürchtung, dass das Reporting die vollständige Ablösung der Access-Anwendung erschweren oder verhindern könnte.

Diese Befürchtung hat sich nicht bestätigt.

Die Analyse zeigt:

- keine kritischen Reporting-Abhängigkeiten
- keine komplexen Reporting-Frameworks
- keine umfangreichen VBA-Landschaften
- keine nicht nachvollziehbaren Spezialmechanismen

Die vorhandenen Berichte basieren überwiegend auf:

- Listen
- Ehrungsvorschlägen
- Kontrollauswertungen
- einfachen Statistiken

---

# 7. Gesamtergebnis

Die vollständige Ablösung des Access-Reportings erscheint realistisch.

Die größte Herausforderung liegt nicht in der Berichtserzeugung selbst, sondern in:

- der Dokumentation der Fachlogik
- der Konsolidierung ähnlicher Berichte
- der Entflechtung historischer Querystrukturen
- der Gestaltung einer zukünftigen Reporting- und Auswertungsplattform

Die Discovery hat gezeigt, dass eine schrittweise Migration Bericht für Bericht sinnvoll und realistisch umsetzbar ist.

---

# 8. Empfehlung für die nächste Phase

Empfohlen wird die Durchführung von:

```text
BL-012b Reportingstrategie
```

Schwerpunkte:

- Zielarchitektur
- Reportingplattform
- PDF-Strategie
- Excel-/CSV-Strategie
- Dokumentgenerierung
- Konsolidierungspotential
- Rollen- und Berechtigungskonzept
- langfristige Erweiterbarkeit
- Berücksichtigung zukünftiger Anforderungen aus EMC Finanzen

---

# Abschlussbewertung

BL-012a wurde erfolgreich abgeschlossen.

Die bestehende Reporting-Landschaft wurde fachlich und technisch analysiert.

Die Grundlage für eine belastbare Reportingstrategie ist geschaffen.
