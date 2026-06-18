# BL-015 – Teststrategie und Qualitätssicherung

Status: In Arbeit

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Hoch

---

# 1. Ziel

Definition und schrittweise Weiterentwicklung einer einheitlichen Teststrategie für die EMC Mitgliederverwaltung.

Die Teststrategie soll sowohl Backend als auch Frontend umfassen und sicherstellen, dass neue Funktionen, Fehlerkorrekturen und Refactorings mit einem angemessenen Maß an Qualitätssicherung umgesetzt werden können.

Ziel ist nicht die maximale Testabdeckung, sondern eine pragmatische und nachhaltige Teststrategie für ein Ein-Personen-Projekt.

---

# 2. Ausgangssituation

Für Backend und Frontend existieren bereits automatisierte Tests.

## Backend

Vorhanden sind bereits:

- Unit Tests
- Repository Tests
- Controller Tests
- Integrationstests
- Security-bezogene Tests

Der Umfang ist historisch gewachsen und bislang nicht durch eine übergreifende Teststrategie definiert.

---

## Frontend

Vorhanden sind bereits:

- Vitest-basierte Unit Tests
- Validator-Tests
- Helper-Tests
- Formularvalidierungen

Aktueller Umfang:

```text
64 erfolgreiche Unit Tests
```

Auch hier existiert bislang keine formalisierte Teststrategie.

---

# 3. Motivation

Mit zunehmendem Funktionsumfang steigen die Risiken bei:

- Fehlerkorrekturen
- Refactorings
- neuen Fachfunktionen
- Reporting-Erweiterungen
- zukünftiger Access-Ablösung

Eine definierte Teststrategie soll sicherstellen, dass Qualitätssicherung reproduzierbar und nachvollziehbar erfolgt.

---

# 4. Fachliche Anforderungen

## FA-015-01 Einheitliche Teststrategie

Für Backend und Frontend ist eine gemeinsame Teststrategie zu definieren.

Diese soll dokumentieren:

- welche Testarten verwendet werden
- wann welche Testarten einzusetzen sind
- welche Mindestanforderungen gelten

---

## FA-015-02 Pragmatischer Ansatz

Die Teststrategie muss zur Größe und Komplexität des Projekts passen.

Ziel ist nicht maximale Testabdeckung, sondern ein angemessenes Verhältnis zwischen Nutzen und Aufwand.

---

## FA-015-03 Dokumentierte Qualitätsziele

Die Qualitätsziele der Teststrategie sind nachvollziehbar zu dokumentieren.

---

# 5. Technische Anforderungen

## TA-015-01 Backend-Testanalyse

Der bestehende Backend-Testbestand ist zu analysieren und zu bewerten.

Dabei insbesondere:

- Testarten
- Testabdeckung
- Wartbarkeit
- identifizierte Lücken

---

## TA-015-02 Frontend-Testanalyse

Der bestehende Frontend-Testbestand ist zu analysieren und zu bewerten.

Dabei insbesondere:

- Validierungen
- Hilfsfunktionen
- Formulare
- Komponenten
- Routing

---

## TA-015-03 Zielbild Testarchitektur

Für Backend und Frontend ist ein Zielbild zu definieren.

Zu bewerten sind insbesondere:

- Unit Tests
- Integrationstests
- Komponententests
- API-Tests
- End-to-End-Tests

---

## TA-015-04 Test-Governance

Es ist festzulegen:

- welche Tests verpflichtend sind
- welche Tests optional sind
- wann neue Tests erstellt werden sollen
- wann bestehende Tests angepasst werden müssen

---

# 6. Release-Qualitätssicherung

## QA-015-01 DEV-Releases

Festlegung der Mindestprüfungen vor einem DEV-Release.

Mögliche Bestandteile:

- Build erfolgreich
- Tests erfolgreich
- Healthchecks erfolgreich

---

## QA-015-02 PROD-Releases

Festlegung der Mindestprüfungen vor einem PROD-Release.

Mögliche Bestandteile:

- Build erfolgreich
- Tests erfolgreich
- DEV-Erprobung abgeschlossen
- Dokumentation aktualisiert

---

# 7. Bewertung End-to-End-Tests

Es soll untersucht werden, ob End-to-End-Tests für die EMC Mitgliederverwaltung sinnvoll sind.

Mögliche Technologien:

- Playwright
- Cypress

Zu bewerten sind:

- Nutzen
- Aufwand
- Wartbarkeit
- Eignung für ein Ein-Personen-Projekt

Eine Einführung ist nicht zwingend Bestandteil dieser Anforderung.

---

# 8. Zulässige Sofortmaßnahmen

Kleine und eindeutig sinnvolle Verbesserungen dürfen direkt im Rahmen von BL-015 umgesetzt werden.

Beispiele:

- Korrektur fehlerhafter Tests
- Bereinigung veralteter Tests
- kleinere Ergänzungen bestehender Testfälle
- kleinere Dokumentationsverbesserungen

Größere Testausbauten sind nicht Bestandteil von BL-015 und als Folgeanforderungen zu behandeln.

---

# 9. Nichtziele

Nicht Bestandteil dieser Anforderung sind:

- Einführung einer CI/CD-Pipeline
- Vorgabe einer festen Testabdeckung in Prozent
- vollständige Neuimplementierung vorhandener Tests
- automatische Quality Gates
- Last- und Performancetests
- großflächiger Ausbau des Testbestands

---

# 10. Dokumentationsauswirkungen

## EMC-DOKUMENTATION

Neue bzw. angepasste Dokumentation:

- Teststrategie
- Qualitätsrichtlinien
- Entwicklungsrichtlinien

---

## Backend Repository

Mögliche Anpassungen:

- README
- Entwicklerdokumentation

---

## Frontend Repository

Mögliche Anpassungen:

- README
- Entwicklerdokumentation

---

# 11. Abnahmekriterien

Die Anforderung gilt als erfüllt, wenn:

- der bestehende Testbestand analysiert wurde
- eine dokumentierte Teststrategie vorliegt
- ein Zielbild für Backend und Frontend definiert wurde
- Release-Mindestanforderungen definiert wurden
- Empfehlungen zu End-to-End-Tests vorliegen
- notwendige Folgearbeiten identifiziert wurden
- ein Ergebnisdokument erstellt wurde

---

# 12. Risiken

Mögliche Risiken:

- übermäßiger Testaufwand
- Wartungsaufwand für Tests
- geringe Akzeptanz komplexer Testverfahren
- Einführung von Werkzeugen mit hohem Pflegeaufwand

Die Strategie soll diese Risiken berücksichtigen.

---

# 13. Folgeanforderungen

Aus der Analyse sind konkrete Folgeanforderungen abzuleiten.

Für jede identifizierte Maßnahme sind zu dokumentieren:

- Titel
- Zielsetzung
- Nutzen
- geschätzter Aufwand
- Priorität

Die Folgeanforderungen sind als Vorschläge für neue Backlog-Einträge zu formulieren.

Beispiele möglicher Folgeanforderungen:

```text
BL-021 Backend Integrationstest-Ausbau

BL-022 Frontend Komponententests

BL-023 API-Teststrategie

BL-024 End-to-End-Tests

BL-025 Release Quality Gate
```

Die tatsächlichen Folgeanforderungen ergeben sich aus der Analyse und sind nicht vorgegeben.

---

# 14. Erwartetes Ergebnis

Nach Abschluss von BL-015 existiert:

- eine dokumentierte Teststrategie
- ein Zielbild für Backend und Frontend
- definierte Qualitätsanforderungen
- definierte Release-Prüfungen
- Empfehlungen zu End-to-End-Tests
- eine priorisierte Liste konkreter Folgeanforderungen

Die Teststrategie dient als Grundlage für alle zukünftigen Entwicklungsphasen.

---

Anforderungs-ID: BL-015

Titel: Teststrategie und Qualitätssicherung

Status: In Arbeit

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Hoch
