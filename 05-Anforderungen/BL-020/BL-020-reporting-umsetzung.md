# BL-020 Reporting Umsetzung

## Status

In Arbeit

## Roadmap-Bezug

P3 Reporting und Auswertung

## Priorität

Hoch

---

# Zielsetzung

Ziel dieser Anforderung ist die schrittweise Ablösung der Access-basierten Reportingfunktionen der EMC Mitgliederverwaltung durch Reportingfunktionen der Service-Welt.

Die Umsetzung erfolgt iterativ in mehreren fachlich abgegrenzten Phasen.

Jede Phase wird separat analysiert, spezifiziert, umgesetzt und dokumentiert.

---

# Ausgangssituation

Im Rahmen von BL-012 Reportingstrategie wurde die bestehende Reportinglandschaft analysiert und eine Zielarchitektur definiert.

Die Analyse zeigte, dass die vollständige Migration des Reportings grundsätzlich möglich ist.

Gleichzeitig wurde empfohlen, die Umsetzung schrittweise vorzunehmen.

---

# Umsetzungsphasen

Die nachfolgende Phasengliederung dient der fachlichen
Strukturierung der Reporting-Anforderungen.

Die Phasen 1 bis 3 wurden in dieser Reihenfolge umgesetzt
und mit Release v1.2.0 produktiv gesetzt.

Für die noch offenen Phasen 4 bis 8 stellt die Nummerierung
weder eine Priorisierung noch eine verbindliche
Umsetzungsreihenfolge dar.

Die Reihenfolge und der konkrete Umfang weiterer
Umsetzungen werden im Rahmen der Projektplanung
gesondert festgelegt.

## Phase 1 – Reporting-Grundgerüst

**Status:** Abgeschlossen

### Ziel

Aufbau der technischen und fachlichen Basis für zukünftige Reports.

### Inhalte

- Reporting-Menü
- Reporting-Architektur
- Berechtigungskonzept
- Exportgrundlagen
- API-Struktur
- gemeinsame Filter- und Selektionsmechanismen
- gemeinsame Reportdarstellung

### Ergebnis

Wiederverwendbares Fundament für alle weiteren Reportingfunktionen.

---

## Phase 2 – Mitgliederverzeichnis

**Status:** Abgeschlossen (erster Umsetzungsumfang)

### Ziel

Schrittweise Migration der bestehenden Mitgliederlisten.

### Ursprünglich vorgesehene Inhalte

- Mitgliederverzeichnis
- kompakte Mitgliederliste
- fortlaufende Mitgliederliste
- chorbezogene Mitgliederlisten

### Umgesetzter Umfang

- Mitgliederverzeichnis als PDF-Bericht im Querformat
- Integration in die gemeinsame Reporting-Architektur
- Bereitstellung über Backend und Frontend
- Berücksichtigung der definierten Berechtigungen
- Integration in die Berichtsauswahl

### Ergebnis

Der erste fachlich abgegrenzte Umsetzungsumfang wurde
erfolgreich implementiert und getestet.

Das Mitgliederverzeichnis ist seit dem Release `v1.2.0`
vom 26.10.2026 produktiv verfügbar.

Weitere ursprünglich vorgesehene Listenvarianten
sind nicht Bestandteil dieses Releases und bleiben
für die spätere Reporting-Planung vorgemerkt.

---

## Phase 3 – Geburtstagswesen

**Status:** Abgeschlossen (erster Umsetzungsumfang)

### Ziel

Schrittweise Migration der Geburtstagsauswertungen.

### Ursprünglich vorgesehene Inhalte

- Geburtstagsliste
- Altersberechnung
- Jubiläumsgeburtstage

### Umgesetzter Umfang

- Geburtstagsliste als PDF-Bericht im Hochformat
- Altersberechnung
- Farbliche Hervorhebung runder Geburtstage
- Berücksichtigung unbekannter Geburtsdaten
- Integration in die gemeinsame Reporting-Architektur
- Integration in die Berichtsauswahl im Frontend

### Ergebnis

Der erste fachlich abgegrenzte Umsetzungsumfang wurde
erfolgreich implementiert und getestet.

Die Geburtstagsliste ist seit dem Release `v1.2.0`
vom 26.10.2026 produktiv verfügbar.

Weitergehende Auswertungen des Geburtstagswesens
können in späteren Reporting-Phasen ergänzt werden.

---

## Phase 4 – Zugehörigkeitswesen

**Status:** Offen

### Ziel

Auswertung von Vereins- und Chorzugehörigkeiten.

### Inhalte

- Zugehörigkeitsjahre
- Jubiläumsjahre
- Kandidatenlisten

### Hinweis

Diese Phase dient ausschließlich der Auswertung von Zugehörigkeiten.

Die eigentliche Ehrungsverwaltung bleibt Bestandteil von BL-009 Ehrungen.

---

## Phase 5 – Kontrollberichte

**Status:** Offen

### Ziel

Migration administrativer Kontroll- und Prüfberichte.

---

## Phase 6 – Mitgliederdatenblatt

**Status:** Offen

### Ziel

Migration des Mitgliederdatenblatts.

---

## Phase 7 – Statistik

**Status:** Offen

### Ziel

Migration statistischer Auswertungen.

---

## Phase 8 – Dokumentgenerierung

**Status:** Offen

### Ziel

Ablösung dokumentbasierter Access-Funktionen.

### Inhalte

- Nutzungsvereinbarungen
- Serienausgaben
- zukünftige Dokumentvorlagen

---

# Abgrenzung

Nicht Bestandteil von BL-020:

- Ehrungsverwaltung
- Funktionenverwaltung
- Verteilerverwaltung

Diese Themen werden in eigenständigen Anforderungen behandelt.

---

# Ergebnisdokumentation

Für jede Phase werden eigene Dokumente unterhalb von:

```text
05-Anforderungen/BL-020/
```

geführt.

Beispiel:

```text
BL-020-phase-01-reporting-grundgeruest.md
BL-020-phase-02-mitgliederverzeichnis.md
BL-020-phase-03-geburtstagswesen.md
...
```

---

# Erfolgskriterien

BL-020 gilt als abgeschlossen, wenn die geplanten Reportingphasen umgesetzt oder bewusst fachlich neu bewertet wurden und die relevanten Access-Reportingfunktionen nicht mehr benötigt werden.
