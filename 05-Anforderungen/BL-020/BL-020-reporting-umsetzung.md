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

## Phase 1 – Reporting-Grundgerüst

**Status:** Abgeschlossen

### Ziel

Aufbau der technischen und fachlichen Basis für zukünftige Reports.

### Inhalte

* Reporting-Menü
* Reporting-Architektur
* Berechtigungskonzept
* Exportgrundlagen
* API-Struktur
* gemeinsame Filter- und Selektionsmechanismen
* gemeinsame Reportdarstellung

### Ergebnis

Wiederverwendbares Fundament für alle weiteren Reportingfunktionen.

---

## Phase 2 – Mitgliederverzeichnis

**Status:** Offen

### Ziel

Migration der bestehenden Mitgliederlisten.

### Inhalte

* Mitgliederverzeichnis
* kompakte Mitgliederliste
* fortlaufende Mitgliederliste
* chorbezogene Mitgliederlisten

---

## Phase 3 – Geburtstagswesen

**Status:** Offen

### Ziel

Migration der Geburtstagsauswertungen.

### Inhalte

* Geburtstagsliste
* Altersberechnung
* Jubiläumsgeburtstage

---

## Phase 4 – Zugehörigkeitswesen

**Status:** Offen

### Ziel

Auswertung von Vereins- und Chorzugehörigkeiten.

### Inhalte

* Zugehörigkeitsjahre
* Jubiläumsjahre
* Kandidatenlisten

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

* Nutzungsvereinbarungen
* Serienausgaben
* zukünftige Dokumentvorlagen

---

# Abgrenzung

Nicht Bestandteil von BL-020:

* Ehrungsverwaltung
* Funktionenverwaltung
* Verteilerverwaltung

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
