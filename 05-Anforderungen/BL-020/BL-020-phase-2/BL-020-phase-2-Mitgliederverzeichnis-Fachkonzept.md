# BL-020 Phase 2 – Mitgliederverzeichnis – Fachkonzept

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 2 – Mitgliederverzeichnis

---

# 1. Zielsetzung

Ziel dieser Phase ist die fachliche Konkretisierung des Reports:

```text
Mitgliederverzeichnis
```

Das Mitgliederverzeichnis ersetzt die bisherige Access-basierte Mitgliederlistenfamilie.

Die bisherigen Berichte:

```text
Mitgliederliste
Mitgliederliste kompakt
Mitgliederliste fortlaufend
Mitgliederliste Kammerchor
```

werden nicht als vier getrennte Berichte übernommen.

Stattdessen entsteht ein gemeinsamer Berichtstyp:

```text
Mitgliederverzeichnis
```

mit konfigurierbaren Varianten.

---

# 2. Ausgangssituation

In Access existieren mehrere Mitgliederlisten mit weitgehend gleicher fachlicher Datenbasis.

Die Unterschiede liegen im Wesentlichen in:

- Umfang der angezeigten Felder
- Gruppierung
- Sortierung
- Layout
- Filterung auf Kammerchor-Mitglieder

Die fachliche Komplexität liegt daher nicht in vier unterschiedlichen Berichtslogiken, sondern in einer gemeinsamen Mitgliederauswertung mit unterschiedlichen Darstellungen.

---

# 3. Fachlicher Grundsatz

Das Mitgliederverzeichnis ist ein lesender Report.

Es dient ausschließlich der Anzeige, Auswertung und Ausgabe von Mitgliederdaten.

Es verändert keine Mitgliederdaten.

Die operative Mitgliederpflege bleibt im Bereich:

```text
Mitglieder
```

Das Mitgliederverzeichnis liegt im Bereich:

```text
Reporting
```

---

# 4. Berichtszweck

Das Mitgliederverzeichnis dient insbesondere:

- als allgemeine Mitgliederübersicht
- als druckbare Mitgliederliste
- als kompakte Arbeitsliste
- als fortlaufende Übersicht ohne Gruppenumbrüche
- als spezielle Liste für Kammerchor-Mitglieder
- als Exportgrundlage für Weiterverarbeitung

---

# 5. Zu konsolidierende Access-Berichte

## 5.1 Mitgliederliste

Bisherige Standardliste.

Fachlicher Zweck:

- vollständige Mitgliederübersicht
- gegliedert nach Status und Stimme
- geeignet für Ausdruck und interne Übersicht

Zukünftige Variante:

```text
Standard
```

---

## 5.2 Mitgliederliste kompakt

Bisherige platzsparende Darstellung.

Fachlicher Zweck:

- reduzierte Feldanzahl
- bessere Lesbarkeit bei vielen Mitgliedern
- kompakter Ausdruck

Zukünftige Variante:

```text
Kompakt
```

---

## 5.3 Mitgliederliste fortlaufend

Bisherige Liste ohne Gruppenumbrüche.

Fachlicher Zweck:

- einfache fortlaufende Übersicht
- geeignet für Suche, Export und Weiterverarbeitung
- keine optische Trennung nach Gruppen

Zukünftige Variante:

```text
Fortlaufend
```

---

## 5.4 Mitgliederliste Kammerchor

Bisherige Sonderliste für Kammerchor-Mitglieder.

Fachlicher Zweck:

- Anzeige nur von Mitgliedern mit Kammerchor-Zuordnung
- Nutzung für kammerchorbezogene Organisation

Zukünftige Variante:

```text
Kammerchor
```

---

# 6. Zielmodell

## 6.1 Gemeinsamer Berichtstyp

```text
Mitgliederverzeichnis
```

## 6.2 Varianten

```text
Standard
Kompakt
Fortlaufend
Kammerchor
```

## 6.3 Grundprinzip

Alle Varianten verwenden dieselbe fachliche Datenbasis.

Unterschiede entstehen durch:

- Filter
- Sortierung
- Gruppierung
- Feldauswahl
- Layout

---

# 7. Datenbasis

Das Mitgliederverzeichnis basiert fachlich auf folgenden Bereichen:

```text
Mitglied
Mitgliedschaft
Kontaktdaten
Lookup-Werte
```

Relevante Tabellen:

```text
tblMitglieder
tblMitgliedschaft
tblKontaktdaten
tblMitgliederstatus_FT
tblStimme_FT
```

---

# 8. Pflichtfelder

Folgende Felder sollen im Mitgliederverzeichnis grundsätzlich verfügbar sein:

```text
Mitgliedsnummer
Nachname
Vorname
Mitgliedsstatus
Stimme
Kammerchor-Kennzeichen
```

Diese Felder bilden die fachliche Mindestbasis für Suche, Sortierung, Filterung und Variantenbildung.

---

# 9. Optionale Felder

Je nach Variante können zusätzlich angezeigt oder exportiert werden:

```text
Anrede
Akademischer Titel
Geburtsdatum
Straße / Hausnummer
Adresszusatz
PLZ
Ort
Telefon privat
Telefon geschäftlich
Mobiltelefon
E-Mail
Eintrittsdatum
Austrittsdatum
Briefanrede
```

Die genaue Sichtbarkeit hängt von Variante und Exportformat ab.

---

# 10. Filter

## 10.1 Pflichtfilter

Für Phase 2 sollen mindestens folgende Filter vorgesehen werden:

```text
Mitgliedsstatus
Stimme
Kammerchor
Freitextsuche
```

---

## 10.2 Mitgliedsstatus

Zweck:

Einschränkung auf bestimmte Mitgliedergruppen.

Beispiele:

```text
Aktives Mitglied
Passives Mitglied
Ehrenmitglied
Ausgeschieden
```

Die verfügbaren Werte stammen aus den vorhandenen Lookup-Daten.

---

## 10.3 Stimme

Zweck:

Einschränkung auf einzelne Stimmgruppen.

Die Stimme besitzt fachliche Relevanz ausschließlich für aktive Sänger.

Verfügbare Stimmen:

```text
1. Tenor
2. Tenor
1. Bass
2. Bass
```

Für andere Mitgliedsstatus ist die Stimme fachlich nicht relevant und soll grundsätzlich den Wert:

```text
keine
```

besitzen.

---

## 10.4 Kammerchor

Zweck:

Einschränkung auf Kammerchor-Mitglieder.

Ausprägungen:

```text
Alle
Nur Kammerchor
Ohne Kammerchor
```

Für die Variante Kammerchor wird dieser Filter automatisch auf:

```text
Nur Kammerchor
```

gesetzt.

---

## 10.5 Freitextsuche

Die Freitextsuche soll mindestens folgende Felder berücksichtigen:

```text
Mitgliedsnummer
Nachname
Vorname
Ort
E-Mail
```

Die Suche dient der schnellen Eingrenzung innerhalb des Reports.

---

# 11. Sortierung

## Fachlicher Grundsatz

Die Sortierung erfolgt innerhalb jeder Gruppierung einheitlich nach:

```text
Nachname
↓
Vorname
```

Dies entspricht dem bisherigen Verhalten der Access-Berichte.

---

## 11.1 Standard

Sortierung innerhalb jeder Gruppe:

```text
Nachname
Vorname
```

---

## 11.2 Kompakt

Sortierung innerhalb jeder Gruppe:

```text
Nachname
Vorname
```

---

## 11.3 Fortlaufend

Sortierung innerhalb jeder Gruppe:

```text
Nachname
Vorname
```

---

## 11.4 Kammerchor

Sortierung innerhalb jeder Stimmgruppe:

```text
Nachname
Vorname
```

---

# 12. Gruppierung

## Fachlicher Grundsatz

Die Gruppierung erfolgt abhängig vom Mitgliedsstatus.

Dabei gilt:

```text
Aktive Mitglieder
→ zusätzliche Gruppierung nach Stimme

Alle anderen Mitgliedsstatus
→ keine Stimmgruppierung
```

Hintergrund:

Die Stimme besitzt fachliche Relevanz ausschließlich für aktive Sänger.

---

## 12.1 Standard

Gruppierung:

```text
Mitgliedsstatus
↓

bei aktiven Mitgliedern zusätzlich:

Stimme
```

Beispiel:

```text
Aktive Mitglieder

  1. Tenor
  ...

  2. Tenor
  ...

  1. Bass
  ...

  2. Bass
  ...

Passive Mitglieder
...

Ehrenmitglieder
...
```

---

## 12.2 Kompakt

Die Gruppierungslogik entspricht der Standardvariante.

---

## 12.3 Fortlaufend

Die Gruppierungslogik entspricht der Standardvariante.

Der Unterschied liegt ausschließlich im Seitenumbruchverhalten.

---

## 12.4 Kammerchor

Da Kammerchor-Mitglieder ausschließlich aktive Mitglieder sein können, erfolgt die Gruppierung direkt nach Stimme.

```text
1. Tenor

2. Tenor

1. Bass

2. Bass
```

---

# 13. Variantenbeschreibung

## Fachlicher Grundsatz

Alle Varianten verwenden dieselbe fachliche Datenbasis.

Die Varianten unterscheiden sich nicht durch unterschiedliche Inhalte, sondern durch:

```text
Layout

Gruppierungsdarstellung

Seitenumbruchverhalten
```

---

## 13.1 Variante Standard

Zweck:

Vollständige Mitgliederliste mit zweizeiliger Darstellung.

Merkmale:

```text
2-zeilige Darstellung

Gruppierung nach Status und Stimme

Automatischer Seitenwechsel bei Gruppierungswechsel
```

---

## 13.2 Variante Kompakt

Zweck:

Platzsparende Mitgliederliste.

Merkmale:

```text
1-zeilige Darstellung

Gruppierung nach Status und Stimme

Automatischer Seitenwechsel bei Gruppierungswechsel
```

---

## 13.3 Variante Fortlaufend

Zweck:

Durchgehende Mitgliederliste ohne Seitenwechsel.

Merkmale:

```text
1-zeilige Darstellung

Gruppierung nach Status und Stimme

Kein automatischer Seitenwechsel bei Gruppierungswechsel
```

---

## 13.4 Variante Kammerchor

Zweck:

Mitgliederliste ausschließlich für Kammerchor-Mitglieder.

Merkmale:

```text
Filter Kammerchor = Ja

Gruppierung nach Stimme

Verwendbar als:
- Standard
- Kompakt
- Fortlaufend
```

---

# 14. Kammerchor-Sonderfälle

## 14.1 Fachlicher Grundsatz

Kammerchor ist kein eigener Mitgliederstatus.

Kammerchor ist ein zusätzliches Merkmal der Mitgliedschaft.

Daher darf Kammerchor nicht mit Statusfiltern vermischt werden.

---

## 14.2 Kombination mit Status

Kammerchor-Mitglieder können fachlich ausschließlich aktive Mitglieder sein.

Daher gilt:

```text
Kammerchor = Ja
→ Mitgliedsstatus = Aktives Mitglied
```

Eine zusätzliche Statusgruppierung ist innerhalb der Kammerchor-Variante nicht erforderlich.

---

## 14.3 Kombination mit Stimme

Die Stimme bleibt im Kammerchor fachlich relevant.

Die Kammerchor-Liste wird daher nach Stimme gruppiert.

Verwendete Stimmen:

```text
1. Tenor
2. Tenor
1. Bass
2. Bass
```

Innerhalb jeder Stimmgruppe erfolgt die Sortierung nach:

```text
Nachname
Vorname
```

---

## 14.4 Layoutvarianten

Für Kammerchor können dieselben Layoutvarianten verwendet werden wie für das allgemeine Mitgliederverzeichnis:

```text
Standard

Kompakt

Fortlaufend
```

Die Unterschiede betreffen ausschließlich Darstellung und Seitenumbruchverhalten.

---

# 15. Datenschutzbewertung

Das Mitgliederverzeichnis enthält personenbezogene Daten.

Besonders relevant sind:

```text
Name
Adresse
Telefonnummern
E-Mail
Geburtsdatum
Mitgliedschaftsdaten
```

Daher gilt:

- Anzeige nur für berechtigte Benutzer
- Phase 2 weiterhin nur für ADMIN
- Exporte besonders restriktiv behandeln
- keine öffentlichen oder anonymen Abrufe
- keine Reportingdaten im Frontend dauerhaft speichern

---

# 16. Exportrelevanz

Das Mitgliederverzeichnis ist grundsätzlich exportfähig.

Relevante Formate:

```text
PDF
Excel
CSV
```

Fachliche Bewertung:

```text
PDF
=
Druck und Weitergabe

Excel
=
Weiterverarbeitung und Sonderauswertung

CSV
=
technischer Austausch und einfache Datenweitergabe
```

Die detaillierte Bewertung erfolgt im separaten Exportkonzept.

---

# 17. Nicht-Ziele der Phase 2

Nicht Bestandteil dieser Phase sind:

- Mitgliederdatenblatt
- Geburtstagsliste
- Zugehörigkeitsauswertungen
- Ehrungsvorschläge
- Statistikberichte
- Dokumentgenerierung
- neue Rollen oder feingranulare Berechtigungen
- Änderung der operativen Mitgliederpflege

---

# 18. Offene fachliche Prüfpunkte für die Umsetzung

Vor der technischen Umsetzung sind folgende Punkte anhand der realen Datenstruktur zu prüfen:

1. Exakte Bezeichnung und technische Ausprägung des Kammerchor-Feldes
2. Vollständige Liste der Mitgliedsstatuswerte
3. Vollständige Liste der Stimmenwerte
4. Gewünschte Standardauswahl aktiver Statusgruppen
5. Ob Telefonnummern in PDF standardmäßig enthalten sein sollen
6. Ob Geburtsdatum im Mitgliederverzeichnis angezeigt werden soll
7. Ob Adresse in Standard- und PDF-Variante enthalten sein soll

---

# 19. Fachliche Entscheidungsempfehlung

Für Phase 2 wird empfohlen:

```text
Mitgliederverzeichnis als gemeinsamer Berichtstyp
mit vier Varianten:
Standard, Kompakt, Fortlaufend, Kammerchor
```

Die erste produktive Umsetzung sollte sich auf folgende Kernfunktionen konzentrieren:

```text
Vorschau
Filter
Sortierung
PDF-Export
Excel-Export
```

CSV kann technisch vorbereitet werden, muss aber fachlich nicht zwingend Bestandteil des ersten Umsetzungsschrittes sein.

---

# 20. Abschlussbewertung

Das Mitgliederverzeichnis eignet sich sehr gut als erster produktiver Reportingbaustein.

Begründung:

- fachlich bekannt
- bestehende Access-Berichte sind gut konsolidierbar
- Datenbasis ist bereits in der Anwendung vorhanden
- hoher praktischer Nutzen
- geringe fachliche Sonderlogik
- gute Grundlage für spätere Reporting-Standards

Damit ist das Mitgliederverzeichnis ein geeigneter Startpunkt für die produktive Ablösung des Access-Reportings.
