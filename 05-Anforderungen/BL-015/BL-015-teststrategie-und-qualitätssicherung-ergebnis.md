# BL-015 – Teststrategie und Qualitätssicherung – Ergebnisdokument

Status: Abgeschlossen

Roadmap-Bezug: P1 Technische Produktreife

Priorität: Hoch

Bearbeitungszeitraum: Juni 2026

---

# 1. Ziel der Anforderung

Ziel von BL-015 war die Analyse des bestehenden Testbestands sowie die Erarbeitung einer langfristig tragfähigen Teststrategie für die EMC Mitgliederverwaltung.

Dabei sollten insbesondere folgende Fragestellungen beantwortet werden:

- Wie gut ist der aktuelle Testbestand?
- Welche Risiken bestehen?
- Welche Testarten sind künftig sinnvoll?
- Welche Qualitätsrichtlinien sollen gelten?
- Welche Folgeanforderungen ergeben sich daraus?

---

# 2. Durchgeführte Analyse

## 2.1 Backend

Der vorhandene Backend-Testbestand wurde analysiert.

Vorhandene Testbereiche:

```text
Controller Tests

Integrationstests

Security Tests

Service Tests

Mapper Tests

Application Tests
```

Untersuchte Testklassen:

```text
AdminUserControllerTest
AuthControllerTest
MemberControllerTest
SystemInfoControllerTest

ActuatorHealthIntegrationTest
AdminUserIntegrationTest
AuthorizationIntegrationTest
AuthSessionIntegrationTest
MemberCreateIntegrationTest
MemberReadIntegrationTest
MemberWriteIntegrationTest
UserLoginStateIntegrationTest

AdminSecurityTest

MemberServiceTest

MemberMapperTest
```

Feststellung:

Der Backend-Testbestand deckt die wesentlichen fachlichen und technischen Kernbereiche bereits gut ab.

Insbesondere Authentifizierung, Autorisierung, Mitgliederverwaltung und Healthchecks sind sinnvoll abgesichert.

---

## 2.2 Frontend

Der vorhandene Frontend-Testbestand wurde analysiert.

Vorhandene Testbereiche:

```text
Validator Tests

Helper Tests

Formularvalidierungen
```

Untersuchte Testdateien:

```text
validationHelpers.test.js
dateHelpers.test.js

stammdatenValidator.test.js
membershipValidator.test.js
contactValidator.test.js
datenschutzValidator.test.js
chorkleidungValidator.test.js
```

Feststellung:

Die fachliche Formular- und Validierungslogik ist bereits gut abgesichert.

Nicht vorhanden sind derzeit:

```text
Komponententests

Navigationstests

Routingtests

End-to-End-Tests
```

---

# 3. Bewertung des Ist-Zustands

## Backend

Bewertung:

```text
Gut
```

Der vorhandene Testbestand wird als tragfähig bewertet.

Ein genereller Ausbau der Backend-Testlandschaft wurde nicht als notwendig eingestuft.

---

## Frontend

Bewertung:

```text
Befriedigend bis Gut
```

Die Validierungslogik ist gut abgesichert.

Größter zukünftiger Ausbaubedarf besteht bei:

```text
Komponententests

Rollenabhängiger Darstellung

kritischen Benutzerabläufen
```

---

# 4. Erarbeitete Soll-Teststrategie

Im Rahmen von BL-015 wurde eine projektweite Soll-Teststrategie definiert.

Dokument:

```text
BL-015-soll-teststrategie-und-test-governance.md
```

Die Soll-Teststrategie definiert:

- Testgrundsätze
- Qualitätsziele
- Testpyramide
- Pflichttests
- empfohlene Tests
- Release-Qualitätssicherung
- Test-Governance

---

# 5. Wesentliche Entscheidungen

## E-015-01 Pragmatische Teststrategie

Die EMC Mitgliederverwaltung verfolgt eine pragmatische Teststrategie.

Nicht Ziel sind:

```text
Coverage-Ziele

Quality Gates

maximale Testabdeckung
```

Ziel ist die Absicherung fachlich relevanter Funktionen mit vertretbarem Aufwand.

---

## E-015-02 Backend-Teststrategie

Der bestehende Backend-Testbestand wird als ausreichend bewertet.

Künftige Erweiterungen erfolgen anlassbezogen und fachlich begründet.

Ein generelles Ausbauprojekt wird nicht verfolgt.

---

## E-015-03 Frontend-Teststrategie

Der Schwerpunkt zukünftiger Erweiterungen liegt im Frontend.

Insbesondere:

```text
Komponententests

Rollen- und Navigationstests
```

wurden als sinnvolle Ausbaubereiche identifiziert.

---

## E-015-04 End-to-End-Strategie

End-to-End-Tests werden grundsätzlich als sinnvoll bewertet.

Es wird jedoch ein bewusst kleiner und wartbarer Ansatz verfolgt.

Playwright wird gegenüber Cypress bevorzugt.

Eine mögliche Einführung erfolgt über eine separate Folgeanforderung.

---

# 6. Release-Qualitätssicherung

Für zukünftige DEV- und PROD-Releases wurden Mindestanforderungen definiert.

Wesentliche Prüfpunkte:

```text
Build erfolgreich

Tests erfolgreich

Healthchecks erfolgreich

Readiness erfolgreich

Versionsprüfung erfolgreich
```

Die detaillierten Vorgaben sind Bestandteil der Soll-Teststrategie.

---

# 7. Abgeleitete Folgeanforderungen

Im Rahmen der Analyse wurden folgende Folgeanforderungen definiert:

---

## BL-021 Frontend Komponententests

Ziel:

Einführung einer gezielten Strategie für Frontend-Komponententests.

Roadmap:

```text
P1 Technische Produktreife
```

---

## BL-022 Reporting-Teststrategie

Ziel:

Definition einer Teststrategie für zukünftige Reportingfunktionen.

Roadmap:

```text
P3 Reporting und Auswertung
```

---

## BL-023 Playwright Smoke Tests

Ziel:

Bewertung und mögliche Einführung einer kleinen Playwright-basierten Smoke-Test-Suite.

Roadmap:

```text
P1 Technische Produktreife
```

---

## BL-024 Entwicklungs- und Qualitätssicherungsprozess

Ziel:

Aufbau einer eigenständigen Prozess- und Arbeitsanleitungsebene für Entwicklung, Tests und Releases.

Roadmap:

```text
P1 Technische Produktreife
```

---

# 8. Dokumentationsauswirkungen

Neu entstanden:

```text
BL-015-soll-teststrategie-und-test-governance.md

BL-021-frontend-komponententests.md

BL-022-reporting-teststrategie.md

BL-023-playwright-smoke-tests.md

BL-024-entwicklungs-und-qualitätssicherungsprozess.md
```

---

# 9. Ergebnisbewertung

Die Analyse zeigt, dass die EMC Mitgliederverwaltung bereits über eine solide Testbasis verfügt.

Ein großflächiger Ausbau der Testlandschaft ist derzeit nicht erforderlich.

Die zukünftige Qualitätssicherung konzentriert sich auf:

```text
Erhalt der bestehenden Testbasis

gezielten Ausbau des Frontends

Reporting-Teststrategie

optionale Smoke Tests

standardisierte Entwicklungs- und Releaseprozesse
```

Damit verfügt das Projekt über eine definierte Teststrategie und eine belastbare Grundlage für die weitere technische Produktreife.

---

# 10. Abschlussstatus

BL-015 wird als erfolgreich abgeschlossen bewertet.

Alle definierten Analyseziele wurden erreicht.

Die Soll-Teststrategie wurde erstellt.

Die notwendigen Folgeanforderungen wurden identifiziert und dokumentiert.

---

Anforderungs-ID: BL-015

Titel: Teststrategie und Qualitätssicherung - Ergebnis

Status: Abgeschlossen
