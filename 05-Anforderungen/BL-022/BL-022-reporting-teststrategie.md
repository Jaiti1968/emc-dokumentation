# BL-022 – Reporting-Teststrategie

Status: Offen

Roadmap-Bezug: P3 Reporting und Auswertung

Priorität: Mittel

---

# 1. Ziel

Definition einer Teststrategie für die zukünftige Reporting-Architektur der EMC Mitgliederverwaltung.

Die Anforderung verfolgt das Ziel, die fachliche Korrektheit, Stabilität und langfristige Wartbarkeit aller Reporting-Funktionen sicherzustellen.

Die Reporting-Teststrategie soll bereits vor der eigentlichen Umsetzung des Reportings definiert werden, um spätere Fachfehler frühzeitig zu vermeiden.

---

# 2. Ausgangssituation

Im Rahmen von BL-012 wurde die bestehende Reporting-Landschaft analysiert und eine zukünftige Reporting-Zielarchitektur definiert.

Die geplante Reporting-Architektur umfasst insbesondere:

- Mitgliederverzeichnis
- Geburtstagslisten
- Ehrungswesen
- Kontrollberichte
- Statistiken
- Dokumentgenerierung
- PDF-Ausgaben
- Excel-Exporte
- CSV-Exporte

Die eigentliche technische Umsetzung erfolgt später im Rahmen von BL-020 Reporting Umsetzung.

Derzeit existiert noch keine projektweite Strategie zur fachlichen Absicherung von Reportingfunktionen.

---

# 3. Motivation

Reportingfunktionen unterscheiden sich von klassischen CRUD-Funktionen.

Viele Fehler entstehen nicht durch technische Probleme, sondern durch fachlich falsche Ergebnisse.

Beispiele:

- falsche Ehrungsberechnung
- fehlerhafte Geburtstagslisten
- unvollständige Mitgliederlisten
- fehlerhafte Statistikwerte
- falsche Selektionen
- fehlerhafte Aggregationen

Solche Fehler werden oft erst spät entdeckt und können zu erheblichen fachlichen Problemen führen.

Eine früh definierte Teststrategie soll dieses Risiko minimieren.

---

# 4. Fachliche Anforderungen

## FA-022-01 Fachliche Korrektheit

Die Teststrategie muss sicherstellen, dass Berichte fachlich korrekte Ergebnisse liefern.

Insbesondere müssen Fachregeln nachvollziehbar und reproduzierbar geprüft werden können.

---

## FA-022-02 Reproduzierbare Referenzdaten

Für Reportingtests sollen definierte Testdatenbestände verwendet werden können.

Diese müssen fachlich nachvollziehbare Ergebnisse erzeugen.

Beispiele:

- Mitglieder mit Ehrungsanspruch
- Mitglieder ohne Ehrungsanspruch
- Geburtstage in verschiedenen Zeiträumen
- Sonderfälle bei Mitgliedschaften

---

## FA-022-03 Fachliche Grenzfälle

Die Teststrategie muss explizit die Prüfung von Grenzfällen berücksichtigen.

Beispiele:

- Stichtagsberechnungen
- Jubiläumsjahre
- Altersgrenzen
- Eintritts- und Austrittsdaten
- Mehrfachzuordnungen

---

## FA-022-04 Nachvollziehbarkeit

Für fachlich relevante Berichte muss nachvollziehbar sein, welche Regeln zu einem Ergebnis geführt haben.

---

# 5. Technische Anforderungen

## TA-022-01 Reporting-Schichten

Die zukünftige Reportingarchitektur ist hinsichtlich ihrer Testbarkeit zu bewerten.

Zu betrachten sind insbesondere:

```text
Reporting Services

Filterlogik

Selektionen

Aggregationen

Exportkomponenten

Dokumentgeneratoren
```

---

## TA-022-02 Testebenen

Für jede Reportingkomponente sind geeignete Testebenen festzulegen.

Mögliche Ebenen:

```text
Unit Tests

Service Tests

Integrationstests

Exporttests
```

Nicht jede Komponente muss auf allen Ebenen getestet werden.

---

## TA-022-03 Referenzberichte

Für besonders kritische Berichte sollen Referenzszenarien definiert werden.

Beispiele:

- Ehrungsliste
- Geburtstagsliste
- Mitgliederverzeichnis
- Statistikberichte

---

## TA-022-04 Exportprüfung

Die Teststrategie soll definieren, wie Exportformate geprüft werden.

Zu betrachten:

- PDF
- Excel
- CSV

Dabei steht die fachliche Korrektheit im Vordergrund.

Nicht Bestandteil sind visuelle Layouttests.

---

# 6. Qualitätsanforderungen

## QA-022-01 Reproduzierbarkeit

Alle Reportingtests müssen reproduzierbare Ergebnisse liefern.

---

## QA-022-02 Wartbarkeit

Tests dürfen nicht übermäßig von Layouts oder Ausgabedarstellungen abhängen.

---

## QA-022-03 Fachliche Lesbarkeit

Fachliche Testfälle sollen für spätere Wartung nachvollziehbar dokumentiert werden.

---

# 7. Zulässige Sofortmaßnahmen

Im Rahmen von BL-022 dürfen umgesetzt werden:

- Analyse bestehender Berichte
- Definition von Referenzdatensätzen
- Definition von Testregeln
- Erstellung von Teststandards
- Dokumentation der Teststrategie

Eine technische Reportingumsetzung erfolgt nicht im Rahmen von BL-022.

---

# 8. Nichtziele

Nicht Bestandteil dieser Anforderung sind:

- Umsetzung von Berichten
- Entwicklung von Reportingservices
- Einführung neuer Exportformate
- Erstellung produktiver PDF-Layouts
- Migration bestehender Access-Berichte

Diese Themen sind Bestandteil von BL-020.

---

# 9. Dokumentationsauswirkungen

## EMC-DOKUMENTATION

Neue bzw. angepasste Dokumentation:

- Reporting-Teststrategie
- Qualitätsrichtlinien
- Reporting-Governance

---

## Entwicklerdokumentation

Dokumentation von:

- Referenzdatensätzen
- Testkonventionen
- Reporting-Testfällen

---

# 10. Abnahmekriterien

Die Anforderung gilt als erfüllt, wenn:

- die Reporting-Teststrategie definiert wurde
- relevante Reporting-Schichten identifiziert wurden
- Referenzszenarien definiert wurden
- Testebenen festgelegt wurden
- Exportprüfungen beschrieben wurden
- die Dokumentation aktualisiert wurde

---

# 11. Risiken

Mögliche Risiken:

- fachlich falsche Berichte trotz technisch erfolgreicher Ausführung
- unvollständige Testdatenbestände
- komplexe Fachregeln
- hohe Wartungskosten bei übermäßig detaillierten Tests

Die Strategie soll einen pragmatischen und wartbaren Ansatz verfolgen.

---

# 12. Folgeanforderungen

Mögliche spätere Folgeanforderungen:

```text
Reporting Referenzdatensätze

Automatisierte Statistiktests

Dokumentgenerator-Teststandards
```

Diese sind nicht Bestandteil von BL-022.

---

# 13. Erwartetes Ergebnis

Nach Abschluss von BL-022 existiert:

- eine definierte Reporting-Teststrategie
- eine Einordnung der Reporting-Testebenen
- definierte Referenzszenarien
- dokumentierte Testregeln
- eine fachliche Grundlage für BL-020 Reporting Umsetzung

Die EMC Mitgliederverwaltung verfügt damit vor Beginn der Reporting-Umsetzung über ein belastbares Konzept zur Qualitätssicherung fachlicher Auswertungen und Berichte.

---

Anforderungs-ID: BL-022

Titel: Reporting-Teststrategie

Status: Offen

Roadmap-Bezug: P3 Reporting und Auswertung

Priorität: Mittel
