# Projektbacklog – EMC Mitgliederverwaltung

## Inhaltsverzeichnis

- [Projektbacklog – EMC Mitgliederverwaltung](#projektbacklog--emc-mitgliederverwaltung)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [1. Zweck und Nutzung](#1-zweck-und-nutzung)
  - [2. Priorisierte aktive Themen](#2-priorisierte-aktive-themen)
  - [3. Technisches Backlog](#3-technisches-backlog)
    - [3.1 Testing / Qualitätssicherung](#31-testing--qualitätssicherung)
      - [Backend](#backend)
      - [Frontend](#frontend)
    - [3.2 Security Hardening](#32-security-hardening)
      - [Backend](#backend-1)
      - [Frontend](#frontend-1)
    - [3.3 Architektur / Plattform](#33-architektur--plattform)
    - [3.4 Frontend Codequalität / UX](#34-frontend-codequalität--ux)
  - [4. Betrieb / Infrastruktur Backlog](#4-betrieb--infrastruktur-backlog)
    - [Docker / NAS Betriebsstandardisierung](#docker--nas-betriebsstandardisierung)
    - [MariaDB Benutzer / Rechte](#mariadb-benutzer--rechte)
    - [Backup / Storage](#backup--storage)
    - [Deployment / Betrieb](#deployment--betrieb)
    - [Betriebsqualität](#betriebsqualität)
  - [5. Fachliches Backlog](#5-fachliches-backlog)
  - [6. Dokumentations Backlog](#6-dokumentations-backlog)
  - [7. Strategische Themen / Entscheidungen](#7-strategische-themen--entscheidungen)
    - [Sicherheitsmodell](#sicherheitsmodell)
    - [Authentifizierung](#authentifizierung)
    - [Datenhaltung](#datenhaltung)
    - [Deployment](#deployment)
  - [8. Erledigt](#8-erledigt)

---

## 1. Zweck und Nutzung

Dieses Dokument ist die zentrale Projektsteuerung für die EMC Mitgliederverwaltung.

Es bündelt:

- technische TODOs
- betriebliche TODOs
- fachliche Weiterentwicklungen
- Dokumentationsaufgaben
- strategische Entscheidungen

Grundsatz:

> Dieses Dokument ist die zentrale Source of Truth für offene Projektthemen.

Temporäre TODO-Dateien in Frontend- oder Backend-Projekten können während aktiver Entwicklung existieren, sollen aber langfristig hier konsolidiert werden.

---

## 2. Priorisierte aktive Themen

| Priorität | Thema | Bereich | Status |
|---------|--------|--------|--------|
| P1 | NAS Betriebsstandardisierung / Docker Cleanup | Betrieb | offen |
| P1 | Benutzerhandbuch MVP / Pilotbetrieb | Dokumentation | offen |
| P1 | MariaDB Benutzer- und Rechtebereinigung / Standardisierung | Infrastruktur / Security | offen |
| P2 | Security Hardening Phase 4 | Technik | offen |
| P2 | Frontend image-basiertes Deployment prüfen | Betrieb / Deployment | offen |
| P2 | Access-Ablösung operative Pflege | Fachlichkeit | offen |

---

## 3. Technisches Backlog

### 3.1 Testing / Qualitätssicherung

#### Backend

- [ ] zusätzliche Integration Tests für zukünftige neue Fachmodule
  - Ehrungen
  - Funktionen
  - Verteiler

- [ ] Security-Hardening-nahe Integration Tests
  - Session Timeout
  - Account Lockout
  - Passwort Lifecycle Erweiterungen

- [ ] Docker Healthcheck

#### Frontend

- [ ] React Component Tests
- [ ] Frontend Integration Tests

Hinweis:

Frontend Integration Tests sind erst nach Phase 4 Security Hardening sinnvoll, da sich Login-/Session-Verhalten noch ändern kann.

---

### 3.2 Security Hardening

#### Backend

- [ ] Passwortregeln verschärfen
- [ ] Initialpasswort automatisch generieren
- [ ] Passwortwechsel beim Erstlogin erzwingen
- [ ] Passwort Reset Workflow
- [ ] Schutz letzter aktiver ADMIN
- [ ] Session Timeout / Auto Logout
- [ ] Session Invalidierung bei Rollenänderung
- [ ] Fehlversuchszähler
- [ ] temporäre Kontosperre
- [ ] Recovery-Konzept für Admin-Zugang

#### Frontend

- [ ] Session Timeout UX
- [ ] Passwortwechsel Erstlogin UI
- [ ] Passwort Reset UI
- [ ] Behandlung gesperrter Benutzer
- [ ] Passwortstärke-Anzeige

---

### 3.3 Architektur / Plattform

- [ ] PATCH vs PUT für Teilupdates bewerten
- [ ] Swagger / OpenAPI
- [ ] Flyway Migrationen vollständig etablieren
- [ ] MariaDB Service-Accounts standardisieren
- [ ] Audit Logging
- [ ] Änderungsverlauf / Historisierung
- [ ] Session Status Indikator
- [ ] Benutzerprofil / eigenes Passwort ändern

- [ ] Frontend Umgebungsidentifikation standardisieren

Ziel:

Die Anwendung soll die aktive Betriebsumgebung eindeutig und technisch sauber anzeigen.

Aktueller Zustand:

```text
localhost | Proxy → localhost:8080
```

Problem:

- technische Proxy-Information statt fachlicher Umgebungsidentität
- DEV / PROD nicht konsistent als Umgebung sichtbar
- lokale Entwicklung nicht sauber vom Betriebsmodus getrennt

Zielbild:

```text
LOCAL | FE v... | BE v...
DEV   | FE v... | BE v...
PROD  | FE v... | BE v...
```

Mögliche technische Umsetzung:

- Frontend Environment-Konfiguration
- build-time Kennzeichnung
- zentrale Environment Detection
- konsistente Anzeige in der Versions-/Systeminfo


---

### 3.4 Frontend Codequalität / UX

- [ ] weitere gemeinsame UI-Komponenten prüfen
- [ ] AdminUsersPage weiter modularisieren
- [ ] API Fehlerhandling vereinheitlichen
- [ ] React Query Keys zentralisieren
- [ ] bessere Session Ladeanzeige
- [ ] verständlichere Netzwerkfehler
- [ ] Bestätigungsdialoge prüfen
- [ ] konsistente Success-/Error Notifications

---

## 4. Betrieb / Infrastruktur Backlog

### Docker / NAS Betriebsstandardisierung

- [ ] Docker Netzwerk Cleanup
- [ ] historische Default-Netzwerke bereinigen
- [ ] NAS Betriebsstandardisierung abschließen
- [ ] Rechtekonzept Docker Build-Verzeichnisse prüfen

---

### MariaDB Benutzer / Rechte

- [ ] MariaDB Benutzer- und Rechtebereinigung / Standardisierung

Ziel:

- historische Benutzer bereinigen
- technische Service-Accounts standardisieren
- personenbezogene technische Accounts vermeiden
- Least-Privilege-Rechtemodell etablieren

Zu prüfen:

- Backend DEV
- Backend PROD
- Backend TEST
- Backup Service
- phpMyAdmin
- Access Altzugriffe
- technische Administrationszugänge

---

### Backup / Storage

- [ ] Backup Pfad Standardisierung

Aktuell:

```text
/volume1/home/JaitiNissi1968/docker/backups/mariadb
```

Ziel:

```text
/volume1/docker/backups/mariadb
```

- [ ] Archiv Cleanup Strategie

für:

```text
/volume1/docker/build/archive/backend
/volume1/docker/build/archive/frontend
```

Fragen:

- wie viele Artefakte behalten?
- Rotation?
- manuell vs automatisiert?

---

### Deployment / Betrieb

- [ ] Frontend image-basiertes Deployment prüfen

Aktueller Stand:

```text
nginx Container
+ dist per Bind Mount
+ nginx.conf per Bind Mount
```

Mögliche Zielvariante:

```text
eigenes Frontend Image
= nginx + dist + nginx.conf fest eingebaut
```

Ziel:

- weniger Bind-Mount-Probleme
- keine Rechteprobleme durch SCP
- einfacheres Rollback über Image Tags
- Deployment analog zum Backend
- weniger manuelle Dateiverwaltung

Bewertung:

- möglich
- mittelfristig sinnvoll
- Aufwand klein bis mittel
- nicht sofort notwendig

---

### Betriebsqualität

- [ ] PROD Smoke-Test Standard definieren
- [ ] Postman Test Collections versionieren
- [ ] Docker Healthcheck

---

## 5. Fachliches Backlog

Access-Ablösung / Fachlichkeit:

- [ ] Ehrungen
- [ ] Funktionen / Chorrollen
- [ ] Verteiler / Gruppen
- [ ] Berichte / Auswertungen
- [ ] Exportfunktionen
- [ ] Mailversand

Strategisch:

- [ ] finale Access-Ablösung planen
- [ ] Reporting-Strategie klären

---

## 6. Dokumentations Backlog

- [ ] 03 Benutzerhandbuch MVP erstellen
- [ ] Screenshots / Benutzerführung ergänzen
- [ ] Changelog Pflegeprozess etablieren
- [ ] Dokumentations-Versionierung prüfen

---

## 7. Strategische Themen / Entscheidungen

### Sicherheitsmodell

Backend nicht direkt exponieren.

Zugriff:

```text
Client
→ Frontend nginx Proxy
→ Backend API
```

API Tests DEV / PROD erfolgen über Frontend Proxy.

---

### Authentifizierung

Session-basierte Authentifizierung.

Kein JWT.

---

### Datenhaltung

MariaDB als führendes System.

Access aktuell noch teilweise Übergangssystem.

Ziel:

vollständige Access-Ablösung.

---

### Deployment

Deployment über Docker / Portainer auf UGREEN NAS.

Build-Artefakte aktuell:

Backend:

```text
*.jar
```

Frontend:

```text
dist/
```

Archivierung aktiv.

Mittelfristig prüfen:

```text
image-basiertes Frontend Deployment
```

---

## 8. Erledigt

Technik:

- [x] Session-basierte Authentifizierung
- [x] Rollenmodell ADMIN / EDITOR / VIEWER
- [x] Login / Logout / Session Restore
- [x] Admin Benutzerverwaltung
- [x] Viewer ReadOnly
- [x] DTO Harmonisierung LocalDate
- [x] Security Test Grundgerüst
- [x] Frontend Unit Tests (64)
- [x] Versionsanzeige FE / BE
- [x] `/api/system/info`
- [x] Backend Integration Tests (29)

Frontend:

- [x] Mitgliederliste
- [x] Suche / Filter / Pagination
- [x] Detailansicht
- [x] Neuanlage
- [x] AutoSave
- [x] Datenschutz
- [x] Chorkleidung
- [x] Formular Refactoring

Betrieb:

- [x] DEV Deployment validiert
- [x] PROD Deployment validiert
- [x] Deployment Artefakt Archivierung
- [x] Deployment Recovery etabliert
- [x] Monitoring mit Uptime Kuma
- [x] Telegram Alerts
- [x] DEV / TEST / PROD Datenbanktrennung
- [x] dedizierter Backend Test-Datenbankbenutzer

Dokumentation:

- [x] Installation / Betrieb
- [x] Deployment
- [x] Architektur
- [x] Troubleshooting / Backup / Restore
- [x] CHANGELOG