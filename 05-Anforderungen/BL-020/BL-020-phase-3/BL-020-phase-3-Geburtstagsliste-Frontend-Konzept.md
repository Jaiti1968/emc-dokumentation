# BL-020 Phase 3 – Geburtstagsliste

# Frontend-Konzept

Stand: 03.07.2026

Status: Entwurf

Anforderung: BL-020 Reporting

Teilphase: Phase 3 – Geburtstagsliste

---

# Inhaltsverzeichnis

1. Zielsetzung
2. Ausgangssituation
3. Zielbild
4. Erweiterung der Reporting-Oberfläche
5. Berichtsauswahl
6. Browseransicht
7. PDF-Export
8. Wiederverwendung bestehender Komponenten
9. Benutzerführung
10. Fehlerbehandlung
11. Erweiterbarkeit
12. Akzeptanzkriterien

---

# 1. Zielsetzung

Mit Phase 3 wird das Reporting erstmals von einem einzelnen Bericht zu einer allgemeinen Reporting-Oberfläche erweitert.

Das Frontend soll künftig mehrere Berichtstypen unterstützen und dabei so aufgebaut werden, dass zukünftige Berichte ohne größere strukturelle Änderungen ergänzt werden können.

---

# 2. Ausgangssituation

Mit Phase 2 wurde bereits eine Reporting-Seite eingeführt.

Diese unterstützt derzeit ausschließlich das Mitgliederverzeichnis.

Vorhanden sind bereits:

- ReportingPage
- Browseransicht
- PDF-Export
- Filterbereich
- Tabellenansicht
- gemeinsame API-Anbindung

Diese Infrastruktur soll vollständig wiederverwendet werden.

---

# 3. Zielbild

Nach Abschluss dieser Phase unterstützt das Frontend mindestens zwei Berichtstypen:

- Mitgliederverzeichnis
- Geburtstagsliste

Die Reporting-Seite entwickelt sich damit von einer spezialisierten Ansicht zu einer allgemeinen Oberfläche für Berichte.

Die Benutzeroberfläche soll bereits auf weitere Berichtstypen vorbereitet werden.

---

# 4. Erweiterung der Reporting-Oberfläche

Die bestehende Reporting-Seite wird um eine Berichtswahl erweitert.

Beispielsweise:

```text
Bericht

○ Mitgliederverzeichnis

○ Geburtstagsliste
```

Nach Auswahl eines Berichtes werden die jeweils verfügbaren Filter angezeigt.

Die Darstellung des Berichts erfolgt anschließend innerhalb derselben Reporting-Seite.

Ein Wechsel zwischen Berichten soll jederzeit ohne Seitenwechsel möglich sein.

---

# 5. Berichtsauswahl

Die Auswahl des gewünschten Berichts erfolgt über eine zentrale Berichtssteuerung.

Diese bildet die Grundlage für alle zukünftigen Reportingfunktionen.

Beispielsweise:

```text
Bericht

▼ Mitgliederverzeichnis
▼ Geburtstagsliste
```

Die konkrete Darstellung (Dropdown, Karten oder Auswahlfelder) bleibt eine Implementierungsentscheidung.

Wichtig ist ausschließlich eine klare Trennung der unterschiedlichen Berichtstypen.

---

# 6. Browseransicht

Nach Auswahl der Geburtstagsliste wird der Bericht direkt innerhalb der Anwendung angezeigt.

Die Browseransicht orientiert sich inhaltlich am PDF-Bericht.

Angezeigt werden:

- Mitgliederstatus
- Stimme
- Vorname
- Nachname
- Geburtsdatum
- Alter

Die Daten werden nach Monaten gruppiert.

Innerhalb eines Monats erfolgt die chronologische Sortierung.

Die Browseransicht dient der schnellen Recherche und Vorbereitung während des laufenden Geschäftsjahres.

---

## Darstellung runder Geburtstage

Runde Geburtstage werden analog zum PDF farblich hervorgehoben.

Die Darstellung basiert ausschließlich auf dem vom Backend gelieferten Jubiläumskennzeichen.

Eine erneute Berechnung im Frontend erfolgt nicht.

---

## Fehlende Geburtsdaten

Mitglieder ohne bekanntes Geburtsdatum werden ebenfalls dargestellt.

Anstelle des Datums erscheint beispielsweise:

```text
unbek.
```

Die Altersspalte bleibt leer.

Dadurch werden unvollständige Stammdaten unmittelbar sichtbar.

---

# 7. PDF-Export

Der bereits vorhandene PDF-Export wird wiederverwendet.

Nach Auswahl der Geburtstagsliste erzeugt das Frontend den entsprechenden PDF-Bericht.

Der Export erfolgt ohne zusätzliche Benutzerinteraktion.

Browseransicht und PDF basieren auf derselben Datenbasis.

---

# 8. Wiederverwendung bestehender Komponenten

Die vorhandene Reporting-Infrastruktur soll möglichst vollständig genutzt werden.

Insbesondere:

- API-Anbindung
- Ladezustände
- Fehlerbehandlung
- Tabellenlayout
- Filterbereich
- PDF-Download

Neue Komponenten entstehen ausschließlich dort, wo sich eine fachliche Wiederverwendung ergibt.

---

# 9. Benutzerführung

Der Ablauf für den Benutzer ist bewusst einfach gehalten.

1. Reporting öffnen

2. Bericht auswählen

3. Filter festlegen

4. Bericht anzeigen

5. Optional PDF erzeugen

Die Bedienung orientiert sich vollständig an der bereits eingeführten Arbeitsweise des Mitgliederverzeichnisses.

Dadurch entsteht kein zusätzlicher Schulungsaufwand.

---

# 10. Fehlerbehandlung

Kann ein Bericht nicht erzeugt werden, erfolgt die Fehlerbehandlung analog zum Mitgliederverzeichnis.

Beispiele:

- keine Daten gefunden
- Serverfehler
- Kommunikationsfehler

Vorhandene Mechanismen zur Anzeige von Fehlermeldungen werden wiederverwendet.

---

# 11. Erweiterbarkeit

Die Erweiterung auf mehrere Berichtstypen bildet die Grundlage für zukünftige Reports.

Beispiele:

- Zugehörigkeitsliste
- Ehrungsvorschläge
- Datenschutzbericht
- Verteilerlisten
- Altersstatistik

Neue Berichte sollen künftig ausschließlich durch:

- Backend-Erweiterung
- Registrierung des Berichtstyps
- Frontend-Konfiguration

integriert werden können.

Größere Änderungen an der Reporting-Seite sollen dafür nicht mehr erforderlich sein.

---

# 12. Akzeptanzkriterien

Die Anforderung gilt als erfüllt, wenn:

- zwischen mindestens zwei Berichtstypen gewählt werden kann,
- die Geburtstagsliste im Browser korrekt dargestellt wird,
- die Monatsgruppierung übernommen wird,
- Jubiläumsgeburtstage farblich hervorgehoben werden,
- Mitglieder ohne Geburtsdatum gekennzeichnet werden,
- der PDF-Export funktioniert,
- bestehende Frontend-Komponenten weitgehend wiederverwendet werden,
- die Reporting-Seite ohne strukturelle Änderungen für weitere Berichte erweiterbar bleibt.

---

# Zusammenfassung

Mit Phase 3 entwickelt sich das Reporting-Frontend von einer einzelnen Berichtsansicht zu einer allgemeinen Reporting-Oberfläche.

Die bestehende Infrastruktur wird konsequent weiterverwendet und gleichzeitig so erweitert, dass zukünftige Berichte ohne grundlegende Umbauten integriert werden können.

Die Geburtstagsliste dient dabei als Referenzimplementierung für den ersten Bericht mit fachlichen Berechnungen sowie für die Unterstützung mehrerer Berichtstypen innerhalb einer gemeinsamen Benutzeroberfläche.
