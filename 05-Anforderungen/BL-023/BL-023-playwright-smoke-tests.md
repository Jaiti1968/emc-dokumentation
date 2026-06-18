# BL-023 – Playwright Smoke Tests

Status: Offen

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Niedrig

---

# 1. Ziel

Prüfung und mögliche Einführung einer kleinen End-to-End-Smoke-Test-Suite auf Basis von Playwright für die EMC Mitgliederverwaltung.

Die Tests sollen zentrale Benutzerabläufe automatisiert prüfen und als zusätzliches Sicherheitsnetz vor DEV- und PROD-Releases dienen.

Ziel ist ausdrücklich nicht der Aufbau einer umfangreichen End-to-End-Testlandschaft.

---

# 2. Ausgangssituation

Für Backend und Frontend existieren bereits automatisierte Tests.

Das Backend verfügt über Unit-, Controller-, Security- und Integrationstests.

Das Frontend verfügt über Vitest-basierte Tests für:

* Validatoren
* Hilfsfunktionen
* Formularvalidierungen

Nicht automatisiert geprüft werden derzeit vollständige Benutzerabläufe über Frontend, Backend, Authentifizierung, Routing und Datenhaltung hinweg.

---

# 3. Motivation

Bestimmte Fehler können trotz erfolgreicher Backend- und Frontend-Unit-Tests unentdeckt bleiben.

Beispiele:

* Login technisch erfolgreich, aber Weiterleitung fehlerhaft
* Mitgliederliste lädt nicht trotz funktionierendem API-Endpunkt
* Detailseite öffnet nicht korrekt
* Speichern funktioniert im Backend, aber UI zeigt falschen Zustand
* Rollenabhängige Navigation ist fehlerhaft

Eine kleine Smoke-Test-Suite soll solche grundlegenden Fehler frühzeitig sichtbar machen.

---

# 4. Fachliche Anforderungen

## FA-023-01 Kritische Benutzerpfade

Es sollen ausschließlich zentrale Benutzerpfade getestet werden.

Mögliche Szenarien:

```text
Login

Mitgliederliste öffnen

Mitglied suchen

Mitgliederdetail öffnen

Mitglied speichern

Benutzerverwaltung öffnen
```

Die finale Auswahl ist im Rahmen der Umsetzung festzulegen.

---

## FA-023-02 Smoke-Test-Charakter

Die Tests dienen als grobe Funktionsprüfung.

Sie ersetzen keine Unit Tests, Integrationstests oder fachlichen Detailtests.

---

## FA-023-03 Begrenzter Umfang

Der Umfang der Smoke Tests ist bewusst klein zu halten.

Die Test-Suite soll auch langfristig wartbar bleiben.

---

# 5. Technische Anforderungen

## TA-023-01 Technologieentscheidung

Playwright ist als bevorzugtes Werkzeug zu bewerten und bei positiver Entscheidung einzuführen.

Cypress ist nicht vorrangig vorgesehen.

---

## TA-023-02 Testumgebung

Es ist festzulegen, gegen welche Umgebung die Tests ausgeführt werden können.

Mögliche Varianten:

* lokale Entwicklungsumgebung
* DEV-Umgebung
* dedizierte Testumgebung

Eine Ausführung gegen PROD ist nur als sehr eingeschränkter Read-Only-Smoke-Test zu bewerten.

---

## TA-023-03 Testdaten

Für schreibende Tests müssen geeignete Testdaten und Rücksetzstrategien definiert werden.

Zu klären sind insbesondere:

* Testbenutzer
* Testmitglied
* erwartete Ausgangsdaten
* Umgang mit Änderungen
* Wiederholbarkeit der Tests

---

## TA-023-04 Authentifizierung

Die Smoke Tests müssen mit dem bestehenden Login- und Session-Konzept der Anwendung kompatibel sein.

Zu prüfen sind:

* Login
* Session-Aufbau
* geschützte Routen
* Logout oder Session-Ende

---

## TA-023-05 Ausführung

Die Tests sollen lokal reproduzierbar ausführbar sein.

Eine spätere Integration in einen automatisierten Releaseprozess ist möglich, aber nicht Bestandteil dieser Anforderung.

---

# 6. Qualitätsanforderungen

## QA-023-01 Stabilität

Smoke Tests dürfen nicht instabil oder zufällig fehlschlagen.

Flaky Tests sind zu vermeiden.

---

## QA-023-02 Wartbarkeit

Tests sollen robust gegenüber kleineren Layoutänderungen sein.

Sie sollen bevorzugt fachliche Elemente und stabile Selektoren verwenden.

---

## QA-023-03 Aussagekraft

Ein fehlgeschlagener Smoke Test muss schnell erkennen lassen, welcher zentrale Benutzerpfad betroffen ist.

---

# 7. Zulässige Sofortmaßnahmen

Im Rahmen von BL-023 dürfen umgesetzt werden:

* Einrichtung von Playwright
* Erstellung weniger Referenz-Smoke-Tests
* Definition von Testdaten
* Dokumentation der Ausführung
* Anpassung der Entwicklerdokumentation

---

# 8. Nichtziele

Nicht Bestandteil dieser Anforderung sind:

* vollständige End-to-End-Testabdeckung
* browserübergreifende Testmatrix
* visuelle Regressionstests
* Performance-Tests
* Lasttests
* flächendeckende Formularprüfung
* produktiver CI/CD-Zwang
* automatische Quality Gates

---

# 9. Dokumentationsauswirkungen

## EMC-DOKUMENTATION

Mögliche neue bzw. angepasste Dokumentation:

* Teststrategie
* Qualitätssicherungsprozess
* Releaseprüfung

---

## Frontend Repository

Mögliche Anpassungen:

* README
* Entwicklerdokumentation
* Playwright-Testdokumentation
* Testdatenhinweise

---

# 10. Abnahmekriterien

Die Anforderung gilt als erfüllt, wenn:

* Playwright als Werkzeug bewertet wurde
* entschieden wurde, ob Playwright eingeführt wird
* bei Einführung eine minimale Smoke-Test-Suite existiert
* die ausführbaren Szenarien dokumentiert wurden
* Testdaten und Testumgebung beschrieben wurden
* die Dokumentation aktualisiert wurde

---

# 11. Risiken

Mögliche Risiken:

* instabile Tests
* hoher Wartungsaufwand
* schwierige Testdatenpflege
* unklare Umgebungstrennung
* versehentliche Änderungen an produktiven Daten

Die Umsetzung muss diese Risiken durch einen bewusst kleinen und kontrollierten Umfang minimieren.

---

# 12. Folgeanforderungen

Mögliche spätere Folgeanforderungen:

```text
Erweiterte End-to-End-Tests

Release-Smoke-Test-Automatisierung

Read-Only-PROD-Smoke-Tests

Visuelle Regressionstests
```

Diese sind nicht Bestandteil von BL-023.

---

# 13. Erwartetes Ergebnis

Nach Abschluss von BL-023 existiert entweder:

* eine begründete Entscheidung gegen die Einführung von Playwright

oder:

* eine kleine, wartbare Playwright-Smoke-Test-Suite
* definierte Testumgebung
* dokumentierte Testdaten
* dokumentierte Ausführung
* aktualisierte Entwicklerdokumentation

Damit verfügt die EMC Mitgliederverwaltung optional über ein zusätzliches Sicherheitsnetz für zentrale Benutzerabläufe vor Releases.

---

Anforderungs-ID: BL-023

Titel: Playwright Smoke Tests

Status: Offen

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Niedrig
