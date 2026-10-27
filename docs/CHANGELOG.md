# EMC Mitgliederverwaltung – Changelog

Alle relevanten Änderungen an der EMC Mitgliederverwaltung werden in dieser Datei dokumentiert.

Ziel:

- nachvollziehbare technische und fachliche Änderungen
- Unterstützung für Deployment und Betrieb
- strukturierte Versionshistorie
- Support- und Diagnoseunterstützung

---

## Historischer Hinweis

Diese Changelog-Dokumentation wurde nachträglich eingeführt.

Frühere Entwicklungsphasen und Änderungen vor dem ersten dokumentierten Eintrag sind nicht vollständig historisch in dieser Datei erfasst.

Der erste Eintrag markiert den Beginn der strukturierten Änderungsdokumentation für:

- Betrieb
- Deployment
- technische Nachvollziehbarkeit
- Versionierung

---

## Versionsschema

Die Anwendung verwendet ein pragmatisches komponentenbezogenes Versionsschema:

```text
MAJOR.MINOR.PATCH-SNAPSHOT
```

Beispiele:

| Komponente | Beispiele                                   |
| ---------- | ------------------------------------------- |
| Frontend   | `1.0.0-SNAPSHOT`, `1.0.0`, `1.0.1-SNAPSHOT` |
| Backend    | `1.1.1-SNAPSHOT`, `1.1.1`, `1.1.2-SNAPSHOT` |

Bedeutung:

| Bestandteil   | Bedeutung                                         |
| ------------- | ------------------------------------------------- |
| MAJOR         | größere fachliche / architektonische Änderungen   |
| MINOR         | funktionale Erweiterungen                         |
| PATCH         | Fehlerbehebungen / kleinere technische Änderungen |
| SNAPSHOT      | Entwicklungsstand                                 |
| ohne SNAPSHOT | definierter stabiler Stand                        |

---

## Änderungsregeln

Ein neuer Changelog-Eintrag wird erstellt bei:

| Änderung                                         | Versionsänderung             |
| ------------------------------------------------ | ---------------------------- |
| Frontend Funktionalität                          | Frontend                     |
| Frontend technische Änderungen mit Nutzerwirkung | Frontend                     |
| Backend API Änderungen                           | Backend                      |
| Backend funktionale Änderungen                   | Backend                      |
| Datenbank Schemaänderungen                       | Backend                      |
| Security Änderungen                              | Backend                      |
| Deployment-relevante Änderungen                  | je nach betroffenem Bereich  |
| reine Dokumentationsänderungen                   | kein Software-Versionssprung |

Hinweis:

Kleine interne Refactorings ohne operative Relevanz müssen nicht einzeln dokumentiert werden.

---

## Struktur eines Changelog-Eintrags

Jeder Eintrag dokumentiert soweit relevant:

- Versionen
- fachliche Änderungen
- technische Änderungen
- Dokumentationsänderungen
- Deployment Hinweise
- Teststatus
- Datenbankänderungen

---

# Changelog Einträge

---

## [2026-05-22] Einführung Versionsinformationen

### Versionen

| Komponente | Version          |
| ---------- | ---------------- |
| Frontend   | `1.0.0-SNAPSHOT` |
| Backend    | `1.1.1-SNAPSHOT` |

---

### Added

Neue technische Versionsanzeige in der Anwendung.

Frontend zeigt:

- Frontend-Version
- Backend-Version

Backend stellt bereit:

```http
GET /api/system/info
```

Beispiel:

```json
{
  "backendVersion": "1.1.1-SNAPSHOT"
}
```

---

### Technical

| Bereich  | Änderung                                 |
| -------- | ---------------------------------------- |
| Frontend | Version aus `package.json`               |
| Frontend | Build Injection via Vite                 |
| Frontend | `import.meta.env.VITE_APP_VERSION`       |
| Frontend | Anzeige im `EnvironmentBadge`            |
| Backend  | Spring Boot Build Metadata aktiviert     |
| Backend  | `META-INF/build-info.properties` erzeugt |
| Backend  | neuer System Endpoint                    |
| Backend  | neuer Test `SystemInfoControllerTest`    |

---

### Documentation

| Dokument                               | Änderung                         |
| -------------------------------------- | -------------------------------- |
| Backend `README.md`                    | System Endpoint / Build Metadata |
| Frontend `README.md`                   | Versionsanzeige                  |
| `README_DEVELOPER.md`                  | technische Versionsinformationen |
| `02-deployment.md`                     | Smoke Test Versionsprüfung       |
| `04-architektur.md`                    | System API / Architektur         |
| `05-troubleshooting-backup-restore.md` | Fehlerbild Versionsabweichung    |

---

### Deployment Notes

| Prüfung             | Erwartung          |
| ------------------- | ------------------ |
| Frontend erreichbar | Anwendung lädt     |
| Login               | erfolgreich        |
| Frontend-Version    | sichtbar / korrekt |
| Backend-Version     | sichtbar / korrekt |

Beispiel:

```text
FE v1.0.0-SNAPSHOT | BE v1.1.1-SNAPSHOT
```

---

### Tests

| Bereich        | Kommando            |
| -------------- | ------------------- |
| Backend Tests  | `mvn test`          |
| Backend Build  | `mvn clean package` |
| Frontend Tests | `npm test`          |
| Frontend Build | `npm run build`     |

---

### Datenbank

| Thema          | Status |
| -------------- | ------ |
| Schemaänderung | Nein   |
| Datenmigration | Nein   |
| manuelles SQL  | Nein   |

---

## Unreleased

Nächste geplante Änderungen werden hier gesammelt.

---

## [2026-05-25] Backend Testarchitektur abgeschlossen / Projektdokumentation konsolidiert

### Versionen

| Komponente | Version          |
| ---------- | ---------------- |
| Frontend   | `1.0.0-SNAPSHOT` |
| Backend    | `1.1.1-SNAPSHOT` |

---

### Added

- dedizierter technischer Test-Datenbankbenutzer für Backend Integration Tests
- vollständige Backend Integration Test-Infrastruktur
- standardisierte DEV / TEST / PROD Datenbanktrennung in der Projektdokumentation
- neues Infrastruktur-Backlog für MariaDB Benutzer- und Rechtebereinigung

---

### Changed

- Backend Teststrategie von Planungsstand auf umgesetzten Zustand aktualisiert
- Projekt-Dokumentation auf aktuellen Architektur- und Betriebsstand konsolidiert
- Release-Prozess um verpflichtende Dokumentationspflege vor Merge / Deployment ergänzt
- Projektbacklog bereinigt und auf aktive Themen fokussiert

---

### Technical

| Bereich             | Änderung                                                                     |
| ------------------- | ---------------------------------------------------------------------------- |
| Backend             | Integration-Testarchitektur produktionsnah mit echter MariaDB Test-Datenbank |
| Backend             | dedizierter Test-DB Benutzer `emc_backend_test_rw`                           |
| Backend             | vollständige Backend Testsuite erfolgreich validiert                         |
| Infrastruktur       | DEV Deployment erfolgreich validiert                                         |
| Infrastruktur       | Betriebs-/Recovery-Dokumentation konsolidiert                                |
| Projektorganisation | verbindliche Doku-Pflege vor Merge etabliert                                 |

---

### Documentation

| Dokument                               | Änderung                                                                       |
| -------------------------------------- | ------------------------------------------------------------------------------ |
| `README.md`                            | Dokumentstruktur / Projektbeschreibung aktualisiert                            |
| `01-installation-betrieb.md`           | DEV / TEST / PROD Modell, Sicherheits- und Betriebsstand aktualisiert          |
| `02-deployment.md`                     | Release-Prozess / Deployment-Ablauf aktualisiert                               |
| `04-architektur.md`                    | Teststrategie, Security, Datenbankmodell, Infrastruktur-Hardening aktualisiert |
| `05-troubleshooting-backup-restore.md` | Betriebs- und Recovery-Konsistenz aktualisiert                                 |
| `06-projektbacklog.md`                 | Backlog konsolidiert / neue Infrastruktur-Themen ergänzt                       |

---

### Deployment Notes

| Prüfung              | Erwartung          |
| -------------------- | ------------------ |
| DEV Deployment       | erfolgreich        |
| Login                | erfolgreich        |
| Mitgliederfunktionen | funktionsfähig     |
| Monitoring           | unverändert stabil |

Hinweise:

- kein PROD Deployment durchgeführt
- keine Runtime-/Funktionsänderung
- kein Software-Versionssprung erforderlich

---

### Tests

| Bereich                    | Kommando               |
| -------------------------- | ---------------------- |
| Backend Tests              | `mvn test`             |
| Backend Integration Tests  | erfolgreich            |
| Backend Gesamttestsuite    | `66 Tests erfolgreich` |
| DEV Deployment Validierung | erfolgreich            |

---

### Datenbank

| Thema          | Status |
| -------------- | ------ |
| Schemaänderung | Nein   |
| Datenmigration | Nein   |
| manuelles SQL  | Ja     |

Hinweise:

- dedizierter Test-Datenbankbenutzer angelegt
- keine Änderungen an PROD-Daten

---

## [2026-06-12] Backend Healthchecks und Runtime-Umgebungserkennung

### Versionen

| Komponente | Version          |
| ---------- | ---------------- |
| Frontend   | `1.0.0-SNAPSHOT` |
| Backend    | `1.1.2`          |

---

### Added

- Spring Boot Actuator integriert
- Health Endpoint `/actuator/health`
- Liveness Endpoint `/actuator/health/liveness`
- Readiness Endpoint `/actuator/health/readiness`
- Build-Zeitpunkt in Systeminformationen aufgenommen
- Runtime-Umgebungserkennung über Spring Profiles
- Erweiterte Backend-Systeminformationen über `/api/system/info`

---

### Changed

- Backend-Monitoring auf Actuator-basierte Healthchecks erweitert
- Deployment-Prozess um Healthcheck- und Environment-Prüfungen ergänzt
- DEV- und PROD-Umgebungen verwenden explizite Spring-Profile
- SystemInfo-Endpunkt liefert zusätzliche Betriebsinformationen

---

### Technical

| Bereich       | Änderung                                                                   |
| ------------- | -------------------------------------------------------------------------- |
| Backend       | Spring Boot Actuator aktiviert                                             |
| Backend       | Health-, Liveness- und Readiness-Probes eingeführt                         |
| Backend       | Datenbankverfügbarkeit in Readiness integriert                             |
| Backend       | Build-Time über Build Metadata verfügbar                                   |
| Backend       | Environment-Erkennung über `SPRING_PROFILES_ACTIVE`                        |
| Backend       | `/api/system/info` erweitert um Environment, aktive Profile und Build-Zeit |
| Backend       | Security-Konfiguration für öffentliche Health-Endpunkte angepasst          |
| Backend       | zusätzliche Controller-Tests ergänzt                                       |
| Infrastruktur | Uptime Kuma Monitoring für DEV und PROD erweitert                          |
| Infrastruktur | Backend-Monitoring auf Actuator-Healthchecks umgestellt                    |

---

### Documentation

| Dokument                        | Änderung                                                 |
| ------------------------------- | -------------------------------------------------------- |
| `README.md`                     | Backend Healthchecks und SystemInfo dokumentiert         |
| `02-deployment.md`              | Healthchecks, Environment-Prüfung und Monitoring ergänzt |
| EMC-INFRA `stack-inventory.md`  | DEV/PROD Health- und Readiness-Monitoring dokumentiert   |
| EMC-INFRA `infra-governance.md` | Actuator-Monitoring als Betriebsstandard ergänzt         |

---

### Deployment Notes

| Prüfung                      | Erwartung            |
| ---------------------------- | -------------------- |
| `/actuator/health`           | HTTP 200 / Status UP |
| `/actuator/health/readiness` | HTTP 200 / Status UP |
| Backend-Version              | korrekt              |
| Environment DEV              | `DEV`                |
| Environment PROD             | `PROD`               |
| Uptime Kuma Health           | UP                   |
| Uptime Kuma Readiness        | UP                   |

Zusätzliche Hinweise:

- Für DEV ist `SPRING_PROFILES_ACTIVE=dev` erforderlich.
- Für PROD ist `SPRING_PROFILES_ACTIVE=prod` erforderlich.
- Bei Portainer-Stacks müssen neue Environment-Variablen zusätzlich in den Stack-Variablen gepflegt werden.
- Health- und Readiness-Endpunkte sind bewusst ohne Authentifizierung erreichbar.

---

### Tests

| Bereich                     | Kommando               |
| --------------------------- | ---------------------- |
| Backend Tests               | `mvn test`             |
| Backend Build               | `mvn clean package`    |
| Backend Gesamttestsuite     | `69 Tests erfolgreich` |
| DEV Deployment Validierung  | erfolgreich            |
| PROD Deployment Validierung | erfolgreich            |
| Healthchecks                | erfolgreich            |
| Monitoring Validierung      | erfolgreich            |

---

### Datenbank

| Thema          | Status |
| -------------- | ------ |
| Schemaänderung | Nein   |
| Datenmigration | Nein   |
| manuelles SQL  | Nein   |

Hinweise:

- Keine Änderungen am Datenbankschema.
- Readiness berücksichtigt die Erreichbarkeit der MariaDB-Datenbank.

### Offene Folgepunkte

- Frontend-Anforderung zur Anzeige der Backend-Laufzeitumgebung (`LOCAL` / `DEV` / `PROD`) aus `/api/system/info` in das Projektbacklog aufnehmen.

---

## [2026-06-19] Security Hardening und Session-Governance

### Versionen

| Komponente | Version          |
| ---------- | ---------------- |
| Frontend   | `1.1.1-SNAPSHOT` |
| Backend    | `1.1.3-SNAPSHOT` |

---

### Added

- Schutz gegen Deaktivierung des letzten aktiven ADMIN-Benutzers
- Schutz gegen Herabstufung des letzten aktiven ADMIN-Benutzers
- Service-Tests für die Benutzerverwaltungslogik
- Session Timeout Konfiguration
- PROD-spezifische Cookie-Sicherheitskonfiguration über Spring Profile

---

### Changed

- generische technische Fehler werden gegenüber Clients neutral beantwortet
- fachliche HTTP-Fehler werden mit Statuscode und fachlicher Fehlermeldung zurückgegeben
- Session- und Cookie-Governance explizit konfiguriert und dokumentiert
- Backend-Dokumentation auf aktuellen Sicherheitsstand aktualisiert

---

### Fixed

- ResponseStatusException wird nicht mehr als unerwarteter technischer Fehler behandelt
- fachliche Konflikte (z. B. letzter ADMIN) werden korrekt als HTTP 409 zurückgegeben

---

### Technical

| Bereich | Änderung                                                   |
| ------- | ---------------------------------------------------------- |
| Backend | Schutzlogik für letzten aktiven ADMIN implementiert        |
| Backend | UserRepository um ADMIN-Prüfungen erweitert                |
| Backend | AdminUserService um Sicherheitsprüfungen erweitert         |
| Backend | ResponseStatusException Handler ergänzt                    |
| Backend | generische Fehlerausgabe gehärtet                          |
| Backend | Session Timeout auf 30 Minuten konfiguriert                |
| Backend | Cookie-Konfiguration mit HttpOnly und SameSite=Lax ergänzt |
| Backend | `Secure=true` im PROD-Profil eingeführt                    |
| Backend | neuer Test `AdminUserServiceTest`                          |
| Backend | Testsuite auf 73 Tests erweitert                           |

---

### Documentation

| Dokument    | Änderung                                                                 |
| ----------- | ------------------------------------------------------------------------ |
| `README.md` | Security Hardening, Session-Governance und Fehlerbehandlung dokumentiert |
| `README.md` | Flyway-Nutzungsstatus dokumentiert                                       |
| `README.md` | Testumfang und Teststand aktualisiert                                    |

---

### Deployment Notes

| Prüfung              | Erwartung                  |
| -------------------- | -------------------------- |
| DEV Deployment       | erfolgreich                |
| Login                | erfolgreich                |
| Benutzerverwaltung   | funktionsfähig             |
| letzter ADMIN Schutz | aktiv                      |
| Systeminformationen  | korrekt                    |
| Healthchecks         | unverändert funktionsfähig |

Zusätzliche Hinweise:

- DEV Deployment erfolgreich validiert.
- Der Schutz des letzten aktiven ADMIN wurde fachlich im Frontend geprüft.
- Fachliche Konflikte werden korrekt als HTTP 409 zurückgegeben.
- Keine Änderungen an Docker-Compose oder Infrastruktur erforderlich.

---

### Tests

| Bereich                        | Kommando               |
| ------------------------------ | ---------------------- |
| Backend Tests                  | `mvn test`             |
| Backend Build                  | `mvn clean package`    |
| Backend Gesamttestsuite        | `73 Tests erfolgreich` |
| DEV Deployment Validierung     | erfolgreich            |
| Security Hardening Validierung | erfolgreich            |

---

### Datenbank

| Thema          | Status |
| -------------- | ------ |
| Schemaänderung | Nein   |
| Datenmigration | Nein   |
| manuelles SQL  | Nein   |

Hinweise:

- Keine Änderungen am Datenbankschema.
- Keine Migrationen erforderlich.
- Bestehende Benutzerdaten bleiben unverändert.

---

## [2026-10-26] BL-020 Reporting – Produktivsetzung Release v1.2.0

### Versionen

| Komponente | Version |
| ---------- | ------- |
| Frontend   | `1.2.0` |
| Backend    | `1.2.0` |

---

### Added

- BL-020 Phase 1: Reporting-Grundgerüst mit Berechtigungssteuerung und gemeinsamer Berichtsarchitektur.
- BL-020 Phase 2: Mitgliederverzeichnis als PDF-Bericht im Querformat.
- BL-020 Phase 3: Geburtstagsliste als PDF-Bericht im Hochformat.
- Altersberechnung und farbliche Hervorhebung runder Geburtstage in der Geburtstagsliste.
- Auswahl der verfügbaren Berichte im Frontend.

---

### Changed

- Reporting-Funktionalität in Frontend und Backend integriert.
- Gemeinsame PDF-Berichtsstruktur für unterschiedliche Berichtslayouts weiterentwickelt.
- Frontend und Backend von `1.2.0-SNAPSHOT` auf die Release-Version `1.2.0` umgestellt.
- PROD-spezifische Session-Cookie-Konfiguration an den derzeitigen HTTP-Betrieb angepasst.

---

### Fixed

- Anmeldeproblem nach dem Backend-PROD-Deployment behoben.
- Ursache: Session-Cookie mit `Secure=true` wurde beim Zugriff über HTTP nicht an nachfolgende API-Anfragen übertragen.
- `server.servlet.session.cookie.secure=false` im Backend-PROD-Profil gesetzt.

---

### Technical

| Bereich       | Änderung                                                                           |
| ------------- | ---------------------------------------------------------------------------------- |
| Backend       | Reporting-Funktionen für Mitgliederverzeichnis und Geburtstagsliste bereitgestellt |
| Backend       | Gemeinsame PDF-Berichtsarchitektur weiterentwickelt                                |
| Backend       | PROD-Session-Cookie-Konfiguration in `application-prod.properties` angepasst       |
| Frontend      | Reporting-Auswahl und Integration beider PDF-Berichte                              |
| Frontend      | Produktives ARM64-Docker-Image `emc-mitglieder-frontend:1.2.0` erstellt            |
| Infrastruktur | Frontend-Compose-Konfigurationen für DEV und PROD aktualisiert                     |
| Deployment    | Backend- und Frontend-Release mit Git-Tags `v1.2.0` gekennzeichnet                 |

---

### Documentation

| Dokument / Repository | Änderung                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------ |
| BL-020 Phase 1        | Reporting-Grundlagen und Berechtigungskonzept dokumentiert                                                         |
| BL-020 Phase 2        | Mitgliederverzeichnis und technische Umsetzung dokumentiert                                                        |
| BL-020 Phase 3        | Geburtstagsliste und technische Umsetzung dokumentiert                                                             |
| EMC-INFRA             | Frontend-Image-Versionen für DEV und PROD aktualisiert                                                             |
| `CHANGELOG.md`        | Produktivsetzung von Release `v1.2.0` dokumentiert                                                                 |
| `02-deployment.md`    | Deployment-, Release- und Rollback-Verfahren einschließlich Archivstrategie und Reporting-Smoke-Tests aktualisiert |

---

### Deployment Notes

| Prüfung                   | Ergebnis           |
| ------------------------- | ------------------ |
| Backend PROD `v1.2.0`     | Erfolgreich        |
| Frontend PROD `v1.2.0`    | Erfolgreich        |
| Anmeldung                 | Erfolgreich        |
| Mitgliederübersicht       | Erfolgreich        |
| Mitgliederverzeichnis PDF | Erfolgreich        |
| Geburtstagsliste PDF      | Erfolgreich        |
| Uptime Kuma Monitoring    | Alle Monitore grün |
| Rollback-Sicherungen      | Vorhanden          |

Zusätzliche Hinweise:

- Backend vor Frontend produktiv gesetzt.
- Backend-Release `1.1.2` und Frontend-Release `1.1.1` für einen möglichen Rollback gesichert.
- PROD wird derzeit über HTTP betrieben.
- Die Einstellung `server.servlet.session.cookie.secure=false` ist für diesen Betriebszustand erforderlich, solange keine HTTPS-Terminierung für den Browserzugriff eingerichtet ist.
- **Sicherheitshinweis:** Bei einer späteren Umstellung auf HTTPS muss `Secure=true` wieder aktiviert und die Anmeldung erneut getestet werden. Der HTTP-Betrieb sollte auf ein vertrauenswürdiges internes Netzwerk beschränkt bleiben.

---

### Tests

| Bereich                     | Ergebnis             |
| --------------------------- | -------------------- |
| Backend `mvn clean verify`  | 99 Tests erfolgreich |
| Frontend `npm run build`    | Erfolgreich          |
| Docker-Build Frontend ARM64 | Erfolgreich          |
| DEV-Funktionstests          | Erfolgreich          |
| PROD-Anmeldung              | Erfolgreich          |
| PROD-Mitgliederverzeichnis  | Erfolgreich          |
| PROD-Geburtstagsliste       | Erfolgreich          |
| PROD-Monitoring             | Erfolgreich          |

---

### Datenbank

| Thema          | Status                                       |
| -------------- | -------------------------------------------- |
| Schemaänderung | Nicht gesondert bestätigt                    |
| Datenmigration | Nicht gesondert bestätigt                    |
| manuelles SQL  | Im Rahmen des Deployments nicht durchgeführt |

Hinweise:

- Vor der Produktivsetzung wurde der erfolgreiche PROD-Datenbank-Backup-Lauf kontrolliert.
- Eine Datenbankmigration war im dokumentierten Deployment-Ablauf nicht erforderlich.

---

# Vorlage für neue Changelog-Einträge

> Diese Vorlage nach oben unterhalb von `## Unreleased` kopieren und anpassen.

```markdown
## [YYYY-MM-DD] Kurzbeschreibung der Änderung

### Versionen

| Komponente | Version          |
| ---------- | ---------------- |
| Frontend   | `X.Y.Z-SNAPSHOT` |
| Backend    | `A.B.C-SNAPSHOT` |

---

### Added

- neue Funktionen
- neue Endpunkte
- neue Features

---

### Changed

- geändertes Verhalten
- technische Änderungen mit Auswirkungen

---

### Fixed

- Fehlerbehebungen

---

### Technical

| Bereich  | Änderung     |
| -------- | ------------ |
| Frontend | Beschreibung |
| Backend  | Beschreibung |

---

### Documentation

| Dokument           | Änderung     |
| ------------------ | ------------ |
| `README.md`        | Beschreibung |
| `02-deployment.md` | Beschreibung |

---

### Deployment Notes

| Prüfung             | Erwartung |
| ------------------- | --------- |
| Frontend erreichbar | OK        |
| Login               | OK        |
| Versionen sichtbar  | korrekt   |

Zusätzliche Hinweise:

- manuelle Schritte
- besondere Risiken
- Reihenfolge beachten

---

### Tests

| Bereich  | Kommando   |
| -------- | ---------- |
| Backend  | `mvn test` |
| Frontend | `npm test` |

---

### Datenbank

| Thema          | Status    |
| -------------- | --------- |
| Schemaänderung | Ja / Nein |
| Datenmigration | Ja / Nein |
| manuelles SQL  | Ja / Nein |

Hinweise:

- ggf. SQL Scripts
- Restore Empfehlung
```
