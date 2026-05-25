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

| Komponente | Beispiele |
|----------|------------|
| Frontend | `1.0.0-SNAPSHOT`, `1.0.0`, `1.0.1-SNAPSHOT` |
| Backend | `1.1.1-SNAPSHOT`, `1.1.1`, `1.1.2-SNAPSHOT` |

Bedeutung:

| Bestandteil | Bedeutung |
|------------|-----------|
| MAJOR | größere fachliche / architektonische Änderungen |
| MINOR | funktionale Erweiterungen |
| PATCH | Fehlerbehebungen / kleinere technische Änderungen |
| SNAPSHOT | Entwicklungsstand |
| ohne SNAPSHOT | definierter stabiler Stand |

---

## Änderungsregeln

Ein neuer Changelog-Eintrag wird erstellt bei:

| Änderung | Versionsänderung |
|---------|------------------|
| Frontend Funktionalität | Frontend |
| Frontend technische Änderungen mit Nutzerwirkung | Frontend |
| Backend API Änderungen | Backend |
| Backend funktionale Änderungen | Backend |
| Datenbank Schemaänderungen | Backend |
| Security Änderungen | Backend |
| Deployment-relevante Änderungen | je nach betroffenem Bereich |
| reine Dokumentationsänderungen | kein Software-Versionssprung |

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

| Komponente | Version |
|----------|---------|
| Frontend | `1.0.0-SNAPSHOT` |
| Backend | `1.1.1-SNAPSHOT` |

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

| Bereich | Änderung |
|--------|----------|
| Frontend | Version aus `package.json` |
| Frontend | Build Injection via Vite |
| Frontend | `import.meta.env.VITE_APP_VERSION` |
| Frontend | Anzeige im `EnvironmentBadge` |
| Backend | Spring Boot Build Metadata aktiviert |
| Backend | `META-INF/build-info.properties` erzeugt |
| Backend | neuer System Endpoint |
| Backend | neuer Test `SystemInfoControllerTest` |

---

### Documentation

| Dokument | Änderung |
|---------|----------|
| Backend `README.md` | System Endpoint / Build Metadata |
| Frontend `README.md` | Versionsanzeige |
| `README_DEVELOPER.md` | technische Versionsinformationen |
| `02-deployment.md` | Smoke Test Versionsprüfung |
| `04-architektur.md` | System API / Architektur |
| `05-troubleshooting-backup-restore.md` | Fehlerbild Versionsabweichung |

---

### Deployment Notes

| Prüfung | Erwartung |
|--------|-----------|
| Frontend erreichbar | Anwendung lädt |
| Login | erfolgreich |
| Frontend-Version | sichtbar / korrekt |
| Backend-Version | sichtbar / korrekt |

Beispiel:

```text
FE v1.0.0-SNAPSHOT | BE v1.1.1-SNAPSHOT
```

---

### Tests

| Bereich | Kommando |
|--------|----------|
| Backend Tests | `mvn test` |
| Backend Build | `mvn clean package` |
| Frontend Tests | `npm test` |
| Frontend Build | `npm run build` |

---

### Datenbank

| Thema | Status |
|------|--------|
| Schemaänderung | Nein |
| Datenmigration | Nein |
| manuelles SQL | Nein |

---

## Unreleased

Nächste geplante Änderungen werden hier gesammelt.

---

## [2026-05-25] Backend Testarchitektur abgeschlossen / Projektdokumentation konsolidiert

### Versionen

| Komponente | Version |
|----------|---------|
| Frontend | `1.0.0-SNAPSHOT` |
| Backend | `1.1.1-SNAPSHOT` |

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

| Bereich | Änderung |
|--------|----------|
| Backend | Integration-Testarchitektur produktionsnah mit echter MariaDB Test-Datenbank |
| Backend | dedizierter Test-DB Benutzer `emc_backend_test_rw` |
| Backend | vollständige Backend Testsuite erfolgreich validiert |
| Infrastruktur | DEV Deployment erfolgreich validiert |
| Infrastruktur | Betriebs-/Recovery-Dokumentation konsolidiert |
| Projektorganisation | verbindliche Doku-Pflege vor Merge etabliert |

---

### Documentation

| Dokument | Änderung |
|---------|----------|
| `README.md` | Dokumentstruktur / Projektbeschreibung aktualisiert |
| `01-installation-betrieb.md` | DEV / TEST / PROD Modell, Sicherheits- und Betriebsstand aktualisiert |
| `02-deployment.md` | Release-Prozess / Deployment-Ablauf aktualisiert |
| `04-architektur.md` | Teststrategie, Security, Datenbankmodell, Infrastruktur-Hardening aktualisiert |
| `05-troubleshooting-backup-restore.md` | Betriebs- und Recovery-Konsistenz aktualisiert |
| `06-projektbacklog.md` | Backlog konsolidiert / neue Infrastruktur-Themen ergänzt |

---

### Deployment Notes

| Prüfung | Erwartung |
|--------|-----------|
| DEV Deployment | erfolgreich |
| Login | erfolgreich |
| Mitgliederfunktionen | funktionsfähig |
| Monitoring | unverändert stabil |

Hinweise:

- kein PROD Deployment durchgeführt
- keine Runtime-/Funktionsänderung
- kein Software-Versionssprung erforderlich

---

### Tests

| Bereich | Kommando |
|--------|----------|
| Backend Tests | `mvn test` |
| Backend Integration Tests | erfolgreich |
| Backend Gesamttestsuite | `66 Tests erfolgreich` |
| DEV Deployment Validierung | erfolgreich |

---

### Datenbank

| Thema | Status |
|------|--------|
| Schemaänderung | Nein |
| Datenmigration | Nein |
| manuelles SQL | Ja |

Hinweise:

- dedizierter Test-Datenbankbenutzer angelegt
- keine Änderungen an PROD-Daten

---

---

# Vorlage für neue Changelog-Einträge

> Diese Vorlage nach oben unterhalb von `## Unreleased` kopieren und anpassen.

```markdown
## [YYYY-MM-DD] Kurzbeschreibung der Änderung

### Versionen

| Komponente | Version |
|----------|---------|
| Frontend | `X.Y.Z-SNAPSHOT` |
| Backend | `A.B.C-SNAPSHOT` |

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

| Bereich | Änderung |
|--------|----------|
| Frontend | Beschreibung |
| Backend | Beschreibung |

---

### Documentation

| Dokument | Änderung |
|---------|----------|
| `README.md` | Beschreibung |
| `02-deployment.md` | Beschreibung |

---

### Deployment Notes

| Prüfung | Erwartung |
|--------|-----------|
| Frontend erreichbar | OK |
| Login | OK |
| Versionen sichtbar | korrekt |

Zusätzliche Hinweise:

- manuelle Schritte
- besondere Risiken
- Reihenfolge beachten

---

### Tests

| Bereich | Kommando |
|--------|----------|
| Backend | `mvn test` |
| Frontend | `npm test` |

---

### Datenbank

| Thema | Status |
|------|--------|
| Schemaänderung | Ja / Nein |
| Datenmigration | Ja / Nein |
| manuelles SQL | Ja / Nein |

Hinweise:

- ggf. SQL Scripts
- Restore Empfehlung
```