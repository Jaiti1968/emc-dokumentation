# Schritt 1 – Fachliche Kategorisierung der Reportingstruktur

## 1. Grundsatz

Die zukünftigen Reportingfunktionen der EMC Mitgliederverwaltung werden fachlich in Reportfamilien gegliedert.

Ziel ist nicht die 1:1-Übernahme einzelner Access-Berichte, sondern die fachliche Konsolidierung gleichartiger Auswertungen.

Berichte werden künftig verstanden als:

```text
Fachliche Auswertung
+
Filter / Parameter
+
Darstellungsvariante
+
Ausgabeformat
```

Dadurch können mehrere bisher getrennte Access-Berichte künftig als Varianten eines gemeinsamen Berichtstyps umgesetzt werden.

---

## 2. Vorgeschlagene Reportfamilien

## 2.1 Mitgliederwesen

Zweck:

Auswertungen und Listen rund um den Mitgliederbestand.

Enthaltene fachliche Berichte:

- Mitgliederverzeichnis
- kompakte Mitgliederliste
- fortlaufende Mitgliederliste
- chorbezogene Mitgliederliste
- Kammerchor-Liste

Bewertung:

Diese Berichte bilden die erste fachliche Umsetzungsphase nach dem Reporting-Grundgerüst.

Künftige Einordnung:

```text
Reporting
└── Mitglieder
    └── Mitgliederverzeichnis
```

Die unterschiedlichen Listenformen werden nicht als eigene Hauptberichte geführt, sondern als Varianten des Mitgliederverzeichnisses.

---

## 2.2 Geburtstagswesen

Zweck:

Vorbereitung und Auswertung von Geburtstagen.

Enthaltene fachliche Berichte:

- Geburtstagsliste
- Jubiläumsgeburtstage
- Geburtstagsauswertung nach Zeitraum

Besonderheit:

Geburtstagsgratulationen sind keine Ehrungen.

Es erfolgt keine Speicherung in der Ehrungshistorie.

Künftige Einordnung:

```text
Reporting
└── Geburtstage
    └── Geburtstagsliste
```

---

## 2.3 Zugehörigkeitswesen

Zweck:

Auswertung von Vereins- und Chorzugehörigkeiten.

Enthaltene fachliche Berichte:

- Zugehörigkeitsjahre
- Jubiläumsjahre
- Kandidatenlisten für Zugehörigkeiten

Abgrenzung:

Diese Reportfamilie dient ausschließlich der Auswertung von Zugehörigkeiten.

Die eigentliche Ehrungsverwaltung bleibt außerhalb von BL-020 und gehört weiterhin zu einer separaten Anforderung.

Künftige Einordnung:

```text
Reporting
└── Zugehörigkeiten
    └── Zugehörigkeitsauswertung
```

---

## 2.4 Kontrollberichte

Zweck:

Administrative Prüf- und Kontrollauswertungen.

Enthaltene fachliche Berichte:

- Datenschutz-Kontrolle
- Verteiler-Kontrolle
- Chorkleidung-Kontrolle
- Datenqualitätsprüfungen

Bewertung:

Diese Berichte können häufig als filterbare Listenansichten mit Exportfunktion umgesetzt werden.

Nicht jeder Kontrollbericht benötigt zwingend ein eigenes PDF-Layout.

Künftige Einordnung:

```text
Reporting
└── Kontrollberichte
    ├── Datenschutz
    ├── Verteiler
    └── Chorkleidung
```

---

## 2.5 Mitgliederdatenblatt

Zweck:

Ausgabe eines vollständigen Mitgliederdatenblatts für ein einzelnes Mitglied.

Enthaltene fachliche Berichte:

- Mitgliederdatenblatt
- spätere Varianten zur Datenprüfung
- ggf. Ausdruck zur manuellen Kontrolle durch das Mitglied

Besonderheit:

Das Mitgliederdatenblatt ist kein Listenbericht, sondern ein Einzelbericht.

Künftige Einordnung:

```text
Reporting
└── Mitglieder
    └── Mitgliederdatenblatt
```

Alternativ kann das Mitgliederdatenblatt später auch direkt aus der Mitgliederdetailseite heraus erzeugt werden.

---

## 2.6 Statistik

Zweck:

Aggregierte Auswertungen und Kennzahlen.

Enthaltene fachliche Berichte:

- Altersdurchschnitt
- Altersstruktur
- Mitgliederbestand
- Stimmenverteilung
- Eintritts- und Austrittsentwicklung

Bewertung:

Die technische Access-Umsetzung der Altersstruktur wird nicht übernommen.

Die Statistik wird später neu konzipiert.

Künftige Einordnung:

```text
Reporting
└── Statistik
    ├── Mitgliederbestand
    ├── Altersstruktur
    └── Stimmenverteilung
```

---

## 2.7 Dokumentgenerierung

Zweck:

Erzeugung individualisierter Dokumente auf Basis von Mitgliederdaten.

Enthaltene fachliche Dokumente:

- Nutzungsvereinbarung Chorkleidung
- spätere Bescheinigungen
- individualisierte Vorlagen

Bewertung:

Dokumentgenerierung ist fachlich mit Reporting verwandt, technisch aber als eigener Bereich zu behandeln.

Künftige Einordnung:

```text
Reporting
└── Dokumente
    └── Nutzungsvereinbarung Chorkleidung
```

Langfristig kann daraus ein eigener Menüpunkt entstehen, falls mehrere Dokumentarten hinzukommen.

---

# 3. Vorgeschlagene fachliche Menüstruktur

Für die erste Reportinggeneration wird folgende Menüstruktur empfohlen:

```text
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
├── Statistik
│   ├── Mitgliederbestand
│   ├── Altersstruktur
│   └── Stimmenverteilung
│
└── Dokumente
    └── Nutzungsvereinbarung Chorkleidung
```

---

# 4. Priorisierung der Reportfamilien

## Phase 2 – Mitgliederverzeichnis

Priorität:

Hoch

Begründung:

- fachlich sichtbar
- vergleichsweise geringe Komplexität
- gute Grundlage für Export- und Filtermechanismen
- ersetzt mehrere bestehende Access-Berichte

---

## Phase 3 – Geburtstagswesen

Priorität:

Hoch

Begründung:

- regelmäßig fachlich genutzt
- überschaubare Logik
- gut geeignet für PDF- und Exportstrategie

---

## Phase 4 – Zugehörigkeitswesen

Priorität:

Mittel bis Hoch

Begründung:

- technisch ähnlich zum Geburtstagswesen
- fachlich wichtig für spätere Ehrungsvorbereitung
- klare Abgrenzung zur eigentlichen Ehrungsverwaltung erforderlich

---

## Spätere Phasen

- Kontrollberichte
- Mitgliederdatenblatt
- Statistik
- Dokumentgenerierung

Diese Bereiche bleiben vorgesehen, werden aber nicht in Phase 1 umgesetzt.

---

# 5. Ergebnis Schritt 1

Die zukünftige Reportingstruktur wird fachlich in folgende Reportfamilien gegliedert:

```text
Mitgliederwesen
Geburtstagswesen
Zugehörigkeitswesen
Kontrollberichte
Mitgliederdatenblatt
Statistik
Dokumentgenerierung
```

Damit ist die fachliche Grundlage für die nächsten Schritte geschaffen:

1. Navigations- und Menüstruktur
2. Backend-Struktur
3. Frontend-Struktur
4. Exportstrategie
5. Berechtigungskonzept
