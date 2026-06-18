# BL-015 – Soll-Teststrategie und Test-Governance

## 1. Grundsatz

Die Teststrategie der EMC Mitgliederverwaltung folgt einem pragmatischen Ansatz.

Ziel ist nicht maximale Testabdeckung, sondern die zuverlässige Absicherung fachlich und technisch kritischer Funktionen mit vertretbarem Aufwand.

Die Teststrategie muss für ein Ein-Personen-Projekt dauerhaft wartbar bleiben.

---

## 2. Qualitätsziele

Die Tests sollen sicherstellen, dass:

- fachliche Kernlogik stabil bleibt
- Änderungen an Formularen und Validierungen keine verdeckten Fehler erzeugen
- Authentifizierung und Autorisierung zuverlässig funktionieren
- Mitglieder lesen, anlegen und ändern stabil bleibt
- DEV- und PROD-Releases nachvollziehbar geprüft werden können
- zukünftige Fachmodule kontrolliert erweitert werden können

Nicht Ziel ist:

- vollständige Testabdeckung in Prozent
- automatisierte Quality Gates
- flächendeckende End-to-End-Testabdeckung
- technische Überkomplexität

---

## 3. Testpyramide EMC Mitgliederverwaltung

### 3.1 Backend

Für das Backend gilt folgende Priorisierung:

```text
Integrationstests
Controller Tests
Service Tests
Mapper Tests
Security Tests
```

Der bestehende Backend-Testbestand ist bereits tragfähig.

Künftige Backend-Erweiterungen sollen vor allem dort getestet werden, wo fachliche Regeln, Sicherheitslogik oder Datenbankzugriffe betroffen sind.

---

### 3.2 Frontend

Für das Frontend gilt folgende Priorisierung:

```text
Validator Tests
Helper Tests
ausgewählte Komponententests
wenige End-to-End-Szenarien
```

Die vorhandenen Validator- und Helper-Tests bilden die wichtigste Grundlage.

Künftiger Ausbau soll selektiv erfolgen, nicht flächendeckend.

---

## 4. Pflicht- und Optionaltests

### 4.1 Pflichttests Backend

Neue oder angepasste Tests sind verpflichtend bei:

- neuer fachlicher Backend-Logik
- Änderungen an Authentifizierung oder Autorisierung
- neuen oder geänderten REST-Endpunkten
- Änderungen an Mitglieder-Schreiboperationen
- Änderungen an Validierungs- oder Mappinglogik
- sicherheitsrelevanten Änderungen

---

### 4.2 Pflichttests Frontend

Neue oder angepasste Tests sind verpflichtend bei:

- neuen Validatoren
- geänderten Validierungsregeln
- neuen Hilfsfunktionen
- fachlich relevanter Formularlogik
- komplexer Datums-, Pflichtfeld- oder Plausibilitätslogik

---

### 4.3 Empfohlene Tests

Empfohlen, aber nicht immer verpflichtend:

- Komponententests für zentrale Formulare
- Tests für rollenabhängige UI-Logik
- Tests für kritische Navigationspfade
- Tests für Fehlerzustände und Ladezustände

---

### 4.4 Keine Testpflicht

Keine automatische Testpflicht besteht bei:

- reinen Layoutänderungen
- Textänderungen
- einfachen CSS-Anpassungen
- rein dokumentarischen Änderungen
- kleineren Umstrukturierungen ohne fachliche Logikänderung

---

## 5. Backend-Zielbild

Das Backend soll weiterhin schwerpunktmäßig über Integrationstests und Controller-Tests abgesichert werden.

Besonders wichtig sind:

- Login
- Session-Verhalten
- Benutzerverwaltung
- Rollen und Rechte
- Mitglieder lesen
- Mitglieder schreiben
- Healthchecks
- Systeminformationen
- Security-Regeln

Repository-Tests sind nur dann separat notwendig, wenn komplexe Datenbanklogik entsteht, die nicht bereits sinnvoll durch Integrationstests abgedeckt wird.

---

## 6. Frontend-Zielbild

Das Frontend soll weiterhin zuerst die fachliche Formular- und Validierungslogik absichern.

Zusätzlich sollten künftig selektiv Komponententests eingeführt werden für:

- Login
- Mitgliederliste
- Mitgliederdetailseite
- zentrale Formularbereiche
- Benutzerverwaltung
- spätere kritische Fachmodule wie Ehrungen, Funktionen, Verteiler und Reporting

Kein Ziel ist eine vollständige UI-Testabdeckung.

---

## 7. End-to-End-Tests

End-to-End-Tests können sinnvoll sein, sollen aber sehr sparsam eingesetzt werden.

Empfohlen wird perspektivisch Playwright.

Cypress wird derzeit nicht bevorzugt, da Playwright für kleine, gezielte Smoke-Tests schlanker und moderner erscheint.

Sinnvolle E2E-Szenarien wären:

1. Login
2. Mitgliederliste öffnen und suchen
3. Mitgliederdetail öffnen
4. Mitgliedsdaten ändern und speichern
5. Admin-Benutzerverwaltung prüfen

Eine Einführung von End-to-End-Tests ist nicht zwingender Bestandteil von BL-015.

Sie sollte als eigene Folgeanforderung behandelt werden.

---

## 8. Release-Qualitätssicherung

### 8.1 DEV-Release Mindestprüfung

Vor einem DEV-Release sollen mindestens durchgeführt werden:

```text
Backend Build erfolgreich
Backend Tests erfolgreich
Frontend Build erfolgreich
Frontend Tests erfolgreich
Backend Healthcheck erfolgreich
Backend Readiness erfolgreich
Frontend im Browser erreichbar
Versionsanzeige plausibel
```

---

### 8.2 PROD-Release Mindestprüfung

Vor einem PROD-Release sollen zusätzlich geprüft werden:

```text
DEV-Erprobung abgeschlossen
Release-Version korrekt gesetzt
Backend Release Build erfolgreich
Frontend Release Build erfolgreich
Tests erfolgreich
Image-Versionen korrekt
Portainer Stack-Konfiguration geprüft
Healthchecks PROD erfolgreich
Readiness PROD erfolgreich
Dokumentation aktualisiert
Rollback-Artefakte vorhanden
```

---

## 9. Test-Governance bei Änderungen

### Bugfix

Bei einem Bugfix gilt:

- Fehler möglichst durch Test reproduzieren
- Fix umsetzen
- Test muss danach erfolgreich laufen

Wenn kein sinnvoller automatisierter Test möglich ist, muss der manuelle Prüfweg dokumentiert werden.

---

### Neue Fachfunktion

Bei neuen Fachfunktionen gilt:

- fachliche Logik erhält Tests
- Backend-Endpunkte erhalten Controller- oder Integrationstests
- Frontend-Validierungen erhalten Unit Tests
- kritische UI-Abläufe werden mindestens manuell geprüft

---

### Refactoring

Bei Refactorings gilt:

- vorhandene Tests müssen unverändert erfolgreich bleiben
- bei Änderung fachlicher Logik sind Tests anzupassen oder zu ergänzen
- reine Strukturänderungen benötigen keine neuen Tests

---

## 10. Dokumentationsregel

Testergebnisse und relevante Qualitätsentscheidungen werden dort dokumentiert, wo sie fachlich hingehören:

```text
Backend-Testdetails
→ Backend Repository

Frontend-Testdetails
→ Frontend Repository

Projektweite Teststrategie
→ EMC-DOKUMENTATION

Release-Prüfungen
→ EMC-DOKUMENTATION / Arbeitsanleitung Releaseprozess
```

---

## 11. Bewertung

Der bestehende Testbestand ist für den aktuellen Projektstand insgesamt gut.

Der größte Ausbaubedarf liegt nicht im Backend, sondern im Frontend bei ausgewählten Komponenten- und später ggf. End-to-End-Tests.

Ein großflächiger Testausbau ist derzeit nicht erforderlich.

Die empfohlene Strategie lautet daher:

```text
Bestehende Tests erhalten
fachliche Logik konsequent testen
Frontend selektiv ausbauen
E2E nur als kleine Smoke-Test-Suite prüfen
keine Coverage-Ziele einführen
keine CI/CD-Pipeline im Rahmen von BL-015 einführen
```
