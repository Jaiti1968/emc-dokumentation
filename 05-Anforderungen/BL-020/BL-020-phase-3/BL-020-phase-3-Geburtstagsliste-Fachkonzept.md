# BL-020 Phase 3 – Geburtstagsliste

# Fachkonzept

Stand: 03.07.2026

Status: Entwurf

Anforderung: BL-020 Reporting

Teilphase: Phase 3 – Geburtstagsliste

---

# Inhaltsverzeichnis

1. Zielsetzung
2. Ausgangssituation
3. Fachliche Zielsetzung
4. Anwendungsfall
5. Fachliche Anforderungen
6. Fachregeln
7. Filter und Selektion
8. Berichtslayout
9. Ausgabeformate
10. Fachliches Datenmodell
11. Abgrenzung zu anderen Auswertungen
12. Architekturbezug
13. Akzeptanzkriterien

---

# 1. Zielsetzung

Ziel dieser Phase ist die Umsetzung der Geburtstagsliste als zweiter Bericht innerhalb der neuen Reporting-Plattform der EMC Mitgliederverwaltung.

Die Geburtstagsliste dient der organisatorischen Vorbereitung der Geburtstagsgratulationen des Vereins und ersetzt den bisherigen Access-Bericht vollständig.

Gleichzeitig dient diese Phase als Referenzimplementierung für:

- einen Bericht mit fachlichen Berechnungen,
- ein PDF im Hochformat,
- die Erweiterung des Frontends auf mehrere Berichtstypen,
- den weiteren Ausbau der gemeinsamen Reporting-Infrastruktur.

---

# 2. Ausgangssituation

Mit Phase 2 wurde bereits das Mitgliederverzeichnis umgesetzt.

Vorhanden sind bereits:

- Browseransicht
- PDF-Export
- ReportingController
- ReportingService
- ReportingRepository
- gemeinsame DTO-Struktur
- gemeinsame PDF-Infrastruktur

Die Geburtstagsliste baut vollständig auf dieser Infrastruktur auf.

Im Gegensatz zum Mitgliederverzeichnis enthält sie erstmals eigenständige fachliche Berechnungen.

---

# 3. Fachliche Zielsetzung

Die Geburtstagsliste unterstützt den Vorstand bei der Planung der Geburtstagsgratulationen während eines Geschäftsjahres.

Sie dient insbesondere:

- der Übersicht aller Geburtstage eines Jahres,
- der frühzeitigen Planung von Gratulationen,
- der Kennzeichnung besonderer Geburtstage,
- der Vorbereitung von Präsenten entsprechend der Geschäftsordnung.

Die Geburtstagsliste ist ausdrücklich keine Ehrungsliste.

Geburtstagsgratulationen werden nicht historisiert und nicht Bestandteil des Ehrungswesens.

---

# 4. Anwendungsfall

Der Bericht wird üblicherweise zu Beginn eines Geschäftsjahres erstellt.

Während des Jahres dient er als Arbeitsunterlage für Vorstand und Präsident.

Durch die Gruppierung nach Monaten können anstehende Geburtstage schnell gefunden werden.

Besonders gekennzeichnete Jubiläumsgeburtstage ermöglichen die rechtzeitige Vorbereitung der vorgesehenen Präsente.

---

# 5. Fachliche Anforderungen

Der Bericht enthält mindestens folgende Informationen:

- Mitgliederstatus
- Stimme
- Vorname
- Nachname
- Geburtsdatum
- Alter im Geschäftsjahr

Die Darstellung erfolgt gruppiert nach Kalendermonaten.

Innerhalb eines Monats erfolgt die chronologische Sortierung der Geburtstage.

---

# 6. Fachregeln

## FR-01 Geschäftsjahr

Die Geburtstagsliste bezieht sich immer auf ein Geschäftsjahr.

Im Bericht wird daher das Geschäftsjahr angezeigt.

Beispiel:

```text
Geburtstagsliste

Geschäftsjahr 2027
```

Das technische Bezugsdatum wird nicht dargestellt.

---

## FR-02 Altersberechnung

Das angegebene Alter beschreibt das Lebensalter, das ein Mitglied innerhalb des ausgewählten Geschäftsjahres erreicht.

Die Berechnung erfolgt unabhängig vom Erstellungsdatum des Berichts.

Beispiel:

Geburtsjahr:

```text
1980
```

Geschäftsjahr:

```text
2027
```

Ergebnis:

```text
47 Jahre
```

---

## FR-03 Jahresbezogene Differenzberechnung

Die Berechnung der Altersangabe basiert auf einer allgemeinen jahresbezogenen Differenzberechnung.

Diese fachliche Logik soll wiederverwendbar implementiert werden.

Sie bildet zukünftig ebenfalls die Grundlage für:

- Berechnung der Vereinszugehörigkeit
- Ehrungsvorschläge
- weitere jahresbezogene Auswertungen

Dadurch wird die Fachlogik zentralisiert und Mehrfachimplementierungen werden vermieden.

---

## FR-04 Runde Geburtstage

Besonders gekennzeichnet werden Geburtstage:

- ab dem 60. Lebensjahr
- anschließend in Fünfjahresschritten

Beispiele:

- 60
- 65
- 70
- 75
- 80
- 85
- 90
- 95
- 100

Diese Kennzeichnung entspricht der Geschäftsordnung des Vereins.

Sie dient dem Präsidenten als Hinweis, dass für diesen Geburtstag ein Präsent vorzubereiten und zu übergeben ist.

---

## FR-05 Darstellung runder Geburtstage

Die Kennzeichnung erfolgt ausschließlich über die Darstellung der Altersangabe.

Das Alter wird farblich hervorgehoben.

Weitere Kennzeichnungen oder zusätzliche Spalten sind nicht vorgesehen.

Dadurch bleibt die gewohnte Arbeitsweise des bisherigen Access-Berichts erhalten.

---

## FR-06 Monatsgruppierung

Die Mitglieder werden nach Kalendermonaten gruppiert.

Die Monatsgruppierung dient der schnellen Orientierung während des Geschäftsjahres und ist ein wesentliches fachliches Merkmal des Berichts.

---

## FR-07 Sortierung

Die Sortierung erfolgt in folgender Reihenfolge:

1. Monat
2. Tag des Geburtstags
3. Nachname
4. Vorname

Dadurch bleibt die Darstellung eindeutig und reproduzierbar.

---

## FR-08 Fehlendes Geburtsdatum

Mitglieder ohne bekanntes Geburtsdatum werden nicht aus dem Bericht ausgeschlossen.

In der Spalte „Geburtsdatum“ wird stattdessen ein kurzer Hinweis, beispielsweise

```text
unbek.
```

ausgegeben.

Die Altersspalte bleibt in diesem Fall leer.

Dadurch werden gleichzeitig fehlende Stammdaten sichtbar.

---

# 7. Filter und Selektion

Die Geburtstagsliste verwendet dieselben Filtermechanismen wie das Mitgliederverzeichnis.

Unterstützt werden:

- Mitgliederstatus
- Stimme

Ein spezieller Kammerchor-Filter ist für diesen Bericht nicht vorgesehen.

Die Filter sollen optional verwendet werden können.

Standardmäßig werden alle Mitglieder berücksichtigt, die den ausgewählten Mitgliederstatus erfüllen.

---

# 8. Berichtslayout

Die Geburtstagsliste wird im Hochformat erstellt.

Der Bericht verwendet den bereits etablierten Aufbau der Reporting-Plattform:

- Vereinslogo
- Berichtstitel
- Geschäftsjahr
- Tabellenkopf
- Monatsgruppierung
- Seitenfuß
- Seitenzahlen
- Standdatum

Die Darstellung orientiert sich bewusst am bisherigen Access-Bericht, wird jedoch an das neue Layout der Reporting-Plattform angepasst.

---

# 9. Ausgabeformate

Im Rahmen dieser Phase werden folgende Ausgabeformate bereitgestellt:

## Browseransicht

Interaktive Darstellung innerhalb der Reporting-Oberfläche.

---

## PDF

Primäres Ausgabeformat.

Der PDF-Bericht dient als Arbeitsunterlage für Vorstand und Präsident.

---

## Excel

Kein Bestandteil dieser Phase.

Für die Geburtstagsliste besteht derzeit kein fachlicher Bedarf für eine Weiterverarbeitung der Daten.

Sollte zukünftig ein entsprechender Bedarf entstehen, kann der Export problemlos ergänzt werden.

---

## CSV

Kein Bestandteil dieser Phase.

---

# 10. Fachliches Datenmodell

Für jedes Mitglied werden mindestens folgende fachliche Informationen bereitgestellt:

- Mitgliederstatus
- Stimme
- Vorname
- Nachname
- Geburtsdatum
- Geburtstag (Tag)
- Geburtsmonat
- Alter im Geschäftsjahr
- Kennzeichen Jubiläumsgeburtstag

Diese Informationen bilden die gemeinsame fachliche Grundlage für Browseransicht und PDF-Ausgabe.

---

# 11. Abgrenzung zu anderen Auswertungen

Die bestehende Access-Abfrage berechnet zusätzlich Altersklassen:

- bis 25 Jahre
- 26 bis 59 Jahre
- ab 60 Jahre

Diese Berechnungen dienen der Altersstatistik und den Meldungen an den Chorverband.

Sie gehören fachlich nicht zur Geburtstagsliste und werden daher in dieser Phase nicht übernommen.

Eine spätere Wiederverwendung im Rahmen eines Statistikmoduls bleibt ausdrücklich vorgesehen.

---

# 12. Architekturbezug

Die Geburtstagsliste erweitert die bestehende Reporting-Plattform.

Dabei gelten weiterhin die Architekturprinzipien:

- Wiederverwendung vor Duplizierung
- Trennung von Fachlogik und Darstellung
- gemeinsame Datenbasis für Browser und PDF
- keine Berichtslogik innerhalb der Exportkomponenten

Die fachlichen Berechnungen werden zentral im Backend implementiert.

PDF- und Browserausgabe greifen ausschließlich auf bereits aufbereitete Berichtsdaten zu.

---

# 13. Akzeptanzkriterien

Die Anforderung gilt als erfüllt, wenn:

- die Geburtstagsliste im Browser angezeigt werden kann,
- ein PDF im Hochformat erzeugt werden kann,
- das Geschäftsjahr korrekt dargestellt wird,
- das Alter im Geschäftsjahr korrekt berechnet wird,
- runde Geburtstage entsprechend der Geschäftsordnung hervorgehoben werden,
- die Monatsgruppierung korrekt erfolgt,
- die Sortierung innerhalb der Monate reproduzierbar ist,
- Mitglieder ohne Geburtsdatum gekennzeichnet werden,
- die bestehenden Filter für Mitgliederstatus und Stimme unterstützt werden,
- die vorhandene Reporting-Infrastruktur ohne Duplizierung wiederverwendet wird.

---

# Zusammenfassung

Mit der Geburtstagsliste entsteht der zweite Bericht der neuen Reporting-Plattform.

Er erweitert die bisherige Infrastruktur erstmals um wiederverwendbare fachliche Berechnungen und bildet damit die Grundlage für weitere Berichte wie Zugehörigkeitslisten, Ehrungsvorschläge und statistische Auswertungen.
