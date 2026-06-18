# BL-024 – Entwicklungs- und Qualitätssicherungsprozess

Status: Offen

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Mittel

---

# 1. Ziel

Definition und Dokumentation eines standardisierten Entwicklungs- und Qualitätssicherungsprozesses für die EMC Mitgliederverwaltung.

Die Anforderung verfolgt das Ziel, wiederholbare und nachvollziehbare Vorgehensweisen für Entwicklung, Tests, Releases und Qualitätssicherung zu etablieren.

Die Ergebnisse sollen die Grundlage für zukünftige Arbeitsanleitungen und Prozessdokumentationen bilden.

---

# 2. Ausgangssituation

Im Projekt wurden bereits zahlreiche technische Standards eingeführt.

Beispiele:

* Backend-Teststrategie
* Frontend-Teststrategie
* Healthchecks
* Monitoring
* Deploymentprozesse
* Versionsmanagement
* Dokumentationsgovernance

Viele dieser Regeln sind derzeit über verschiedene Dokumente verteilt.

Eine zusammenhängende Prozessdokumentation existiert bislang nicht.

---

# 3. Motivation

Mit zunehmender Projektgröße steigt die Bedeutung einheitlicher Vorgehensweisen.

Wichtige Abläufe sollen nicht ausschließlich durch Erfahrungswissen oder einzelne Projektdokumente beschrieben werden.

Insbesondere folgende Bereiche sollen künftig standardisiert dokumentiert werden:

* Entwicklung
* Tests
* Releases
* Fehlerkorrekturen
* Dokumentationspflege

---

# 4. Fachliche Anforderungen

## FA-024-01 Einheitliche Vorgehensweisen

Für wiederkehrende Tätigkeiten sollen standardisierte Abläufe definiert werden.

Die Abläufe müssen nachvollziehbar, reproduzierbar und wartbar sein.

---

## FA-024-02 Nachvollziehbarkeit

Entwicklungs- und Qualitätssicherungsmaßnahmen sollen dokumentiert werden können.

Dies betrifft insbesondere:

* Anforderungen
* Umsetzungen
* Tests
* Releases
* Fehlerkorrekturen

---

## FA-024-03 Projektweite Anwendbarkeit

Die definierten Prozesse müssen sowohl für Backend als auch Frontend anwendbar sein.

---

# 5. Technische Anforderungen

## TA-024-01 Prozessanalyse

Bestehende Vorgehensweisen sind zu analysieren und zu konsolidieren.

Insbesondere:

```text
Entwicklungsprozess

Testprozess

Releaseprozess

Fehlerbehebungsprozess

Dokumentationsprozess
```

---

## TA-024-02 Prozessstruktur

Es ist eine geeignete Dokumentationsstruktur für Arbeitsanleitungen und Prozessbeschreibungen festzulegen.

Zu bewerten sind insbesondere:

```text
06-Arbeitsanleitungen/

testprozess.md

releaseprozess.md

entwicklungsrichtlinien.md
```

Die endgültige Struktur ist im Rahmen der Analyse festzulegen.

---

## TA-024-03 Testprozess

Auf Basis von BL-015 ist ein standardisierter Testprozess zu definieren.

Zu betrachten sind:

* Backend Tests
* Frontend Tests
* manuelle Prüfungen
* Fehlerbehandlung
* Testdokumentation

---

## TA-024-04 Releaseprozess

Auf Basis der bisherigen Deployment- und Healthcheck-Standards ist ein standardisierter Releaseprozess zu definieren.

Zu betrachten sind:

* DEV-Releases
* PROD-Releases
* Versionsprüfung
* Healthcheck-Prüfung
* Rollbackfähigkeit

---

## TA-024-05 Entwicklungsrichtlinien

Es ist zu bewerten, welche projektweiten Entwicklungsrichtlinien dokumentiert werden sollen.

Mögliche Themen:

* Branch-Nutzung
* Commit-Konventionen
* Dokumentationspflichten
* Testpflichten
* Umgang mit Bugfixes

---

# 6. Qualitätsanforderungen

## QA-024-01 Praktikabilität

Die Prozesse müssen für ein Ein-Personen-Projekt geeignet bleiben.

Übermäßige Bürokratie ist zu vermeiden.

---

## QA-024-02 Wartbarkeit

Die Prozessdokumentation muss langfristig pflegbar bleiben.

---

## QA-024-03 Verständlichkeit

Die Dokumente sollen als praktische Arbeitsanleitungen nutzbar sein.

---

# 7. Zulässige Sofortmaßnahmen

Im Rahmen von BL-024 dürfen umgesetzt werden:

* Analyse bestehender Abläufe
* Konsolidierung vorhandener Regeln
* Erstellung neuer Prozessdokumente
* Anpassung der Dokumentationsstruktur
* Einführung neuer Arbeitsanleitungen

---

# 8. Nichtziele

Nicht Bestandteil dieser Anforderung sind:

* Einführung von CI/CD-Systemen
* Einführung externer Qualitätssicherungssysteme
* Einführung komplexer Freigabeworkflows
* organisatorische Rollenmodelle
* Mehrpersonen-Prozesse

Der Fokus liegt auf pragmatischen Prozessen für die bestehende Projektstruktur.

---

# 9. Dokumentationsauswirkungen

## EMC-DOKUMENTATION

Mögliche neue Dokumente:

```text
06-Arbeitsanleitungen/

testprozess.md

releaseprozess.md

entwicklungsrichtlinien.md
```

Weitere Dokumente können bei Bedarf ergänzt werden.

---

## Bestehende Dokumente

Mögliche Anpassungen:

* Projektsteuerungsdokumente
* Teststrategie
* Deploymentdokumentation
* Governance-Dokumente

---

# 10. Abnahmekriterien

Die Anforderung gilt als erfüllt, wenn:

* bestehende Prozesse analysiert wurden
* eine Zielstruktur definiert wurde
* erforderliche Arbeitsanleitungen erstellt wurden
* Testprozess dokumentiert wurde
* Releaseprozess dokumentiert wurde
* Dokumentation konsolidiert wurde

---

# 11. Risiken

Mögliche Risiken:

* unnötige Bürokratie
* doppelte Dokumentation
* veraltete Prozessbeschreibungen
* zu komplexe Abläufe

Die Umsetzung soll bewusst pragmatisch erfolgen.

---

# 12. Folgeanforderungen

Mögliche spätere Folgeanforderungen:

```text
Erweiterte Entwicklungsstandards

Automatisierte Releaseprüfungen

CI/CD-Governance

Qualitätsmetriken
```

Diese sind nicht Bestandteil von BL-024.

---

# 13. Erwartetes Ergebnis

Nach Abschluss von BL-024 existiert:

* eine definierte Prozessstruktur
* dokumentierte Arbeitsanleitungen
* dokumentierter Testprozess
* dokumentierter Releaseprozess
* konsolidierte Qualitätsrichtlinien

Die EMC Mitgliederverwaltung verfügt damit über eine eigenständige Prozess- und Arbeitsanleitungsebene zwischen Projektsteuerung und technischer Umsetzung.

---

Anforderungs-ID: BL-024

Titel: Entwicklungs- und Qualitätssicherungsprozess

Status: Offen

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Mittel
