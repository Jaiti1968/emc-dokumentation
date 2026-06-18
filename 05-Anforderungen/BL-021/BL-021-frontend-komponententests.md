# BL-021 – Frontend Komponententests

Status: Offen

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Mittel

---

# 1. Ziel

Einführung einer gezielten Strategie für Frontend-Komponententests innerhalb der EMC Mitgliederverwaltung.

Die Anforderung verfolgt nicht das Ziel einer vollständigen UI-Testabdeckung, sondern die Absicherung fachlich und technisch kritischer React-Komponenten.

Komponententests sollen die bestehende Frontend-Testbasis ergänzen und zukünftige Refactorings sowie Funktionserweiterungen sicherer machen.

---

# 2. Ausgangssituation

Im Frontend existieren bereits automatisierte Tests für:

- Validatoren
- Hilfsfunktionen
- Formularvalidierungen

Aktueller Umfang:

```text
64 erfolgreiche Unit Tests
```

Die bestehende Teststrategie deckt die fachliche Validierungslogik bereits gut ab.

Nicht automatisiert getestet werden derzeit:

- React-Komponenten
- Benutzerinteraktionen innerhalb von Komponenten
- Rollenabhängige Darstellung
- Komponentenintegration
- Zustandswechsel innerhalb komplexerer Oberflächen

---

# 3. Motivation

Mit zunehmendem Ausbau der Anwendung steigt die Komplexität des Frontends.

Insbesondere zukünftige Funktionen wie:

- Ehrungen
- Funktionen
- Verteiler
- Reporting
- Dokumentgenerierung

werden zusätzliche Komponenten und Interaktionen erzeugen.

Ohne Komponententests steigt das Risiko, dass Refactorings oder Erweiterungen bestehende Oberflächen unbeabsichtigt beeinträchtigen.

---

# 4. Fachliche Anforderungen

## FA-021-01 Selektiver Ansatz

Komponententests sollen ausschließlich für fachlich oder technisch relevante Komponenten eingeführt werden.

Eine vollständige Testabdeckung aller Komponenten ist nicht Ziel dieser Anforderung.

---

## FA-021-02 Benutzerorientierte Tests

Tests sollen möglichst aus Sicht des Benutzers formuliert werden.

Zu prüfen sind insbesondere:

- Anzeige relevanter Inhalte
- Benutzerinteraktionen
- Zustandswechsel
- Fehlermeldungen
- Rollenabhängige Sichtbarkeit

---

## FA-021-03 Wartbarkeit

Die Teststrategie muss langfristig wartbar bleiben.

Komponententests dürfen keine übermäßige Kopplung an interne Implementierungsdetails erzeugen.

---

# 5. Technische Anforderungen

## TA-021-01 Testframework

Die Komponententests sollen in die bestehende Frontend-Testlandschaft integriert werden.

Zu bewerten sind insbesondere:

- Vitest
- React Testing Library
- bestehende Projektstruktur

---

## TA-021-02 Testkonventionen

Es sind verbindliche Regeln für Komponententests festzulegen.

Insbesondere:

- Namenskonventionen
- Verzeichnisstruktur
- Testaufbau
- Mocking-Strategien

---

## TA-021-03 Zielkomponenten

Für die erste Ausbaustufe sind geeignete Komponenten zu identifizieren.

Mögliche Kandidaten:

```text
LoginForm
ProtectedRoute
MemberList
MemberDetail
UserManagement
```

Die tatsächliche Auswahl ist im Rahmen der Analyse festzulegen.

---

## TA-021-04 Rollen- und Rechteprüfung

Komponententests sollen insbesondere dort eingesetzt werden, wo Rollen oder Berechtigungen die Darstellung beeinflussen.

Beispiele:

- Admin-Funktionen
- Benutzerverwaltung
- geschützte Bereiche

---

# 6. Qualitätsanforderungen

## QA-021-01 Reproduzierbarkeit

Alle Komponententests müssen lokal reproduzierbar ausführbar sein.

---

## QA-021-02 Schnelle Ausführung

Die Testausführung soll weiterhin in kurzer Zeit möglich sein.

Komponententests dürfen die Entwicklerproduktivität nicht erheblich beeinträchtigen.

---

## QA-021-03 Lesbarkeit

Tests sollen als Dokumentation des erwarteten Komponentenverhaltens dienen.

---

# 7. Zulässige Sofortmaßnahmen

Im Rahmen von BL-021 dürfen umgesetzt werden:

- Einrichtung der Testinfrastruktur
- Erstellung erster Referenztests
- Anpassung der Frontend-Dokumentation
- Definition von Teststandards

---

# 8. Nichtziele

Nicht Bestandteil dieser Anforderung sind:

- End-to-End-Tests
- Browserübergreifende Tests
- visuelle Snapshot-Tests
- Performance-Tests
- vollständige UI-Testabdeckung
- Einführung neuer Build- oder Deploymentprozesse

---

# 9. Dokumentationsauswirkungen

## EMC-DOKUMENTATION

Neue bzw. angepasste Dokumentation:

- Teststrategie
- Qualitätsrichtlinien

---

## Frontend Repository

Mögliche Anpassungen:

- README
- Entwicklerdokumentation
- Testkonventionen

---

# 10. Abnahmekriterien

Die Anforderung gilt als erfüllt, wenn:

- eine Komponententeststrategie definiert wurde
- die technische Umsetzung festgelegt wurde
- Testkonventionen dokumentiert wurden
- mindestens eine erste Referenzimplementierung vorliegt
- die Dokumentation aktualisiert wurde

---

# 11. Risiken

Mögliche Risiken:

- übermäßiger Testaufwand
- hohe Wartungskosten
- zu starke Kopplung an Implementierungsdetails
- geringe Akzeptanz durch komplexe Teststrukturen

Die Umsetzung soll diese Risiken minimieren.

---

# 12. Folgeanforderungen

Mögliche spätere Folgeanforderungen:

```text
Frontend UI-Teststandards

Erweiterte Rollen- und Navigationstests

Visuelle Regressionstests
```

Diese sind nicht Bestandteil von BL-021.

---

# 13. Erwartetes Ergebnis

Nach Abschluss von BL-021 existiert:

- eine definierte Komponententeststrategie
- eine technische Zielarchitektur
- dokumentierte Teststandards
- erste produktive Komponententests
- aktualisierte Entwicklerdokumentation

Die EMC Mitgliederverwaltung verfügt damit über eine zusätzliche Qualitätssicherungsebene zwischen Unit Tests und End-to-End-Tests.

---

Anforderungs-ID: BL-021

Titel: Frontend Komponententests

Status: Offen

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Mittel
