# Installations- und Betriebshandbuch

## Inhaltsverzeichnis

- [Installations- und Betriebshandbuch](#installations--und-betriebshandbuch)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [1. Zweck und Geltungsbereich](#1-zweck-und-geltungsbereich)
    - [1.1 Ziel des Dokuments](#11-ziel-des-dokuments)
    - [1.2 Zielgruppe](#12-zielgruppe)
    - [1.3 Abgrenzung zu anderen Dokumenten](#13-abgrenzung-zu-anderen-dokumenten)
  - [2. Systemübersicht](#2-systemübersicht)
    - [2.1 Zweck des Systems](#21-zweck-des-systems)
    - [2.2 Betriebsmodell](#22-betriebsmodell)
    - [2.3 Systemkomponenten](#23-systemkomponenten)
  - [3. Systemvoraussetzungen](#3-systemvoraussetzungen)
    - [3.1 Hardware](#31-hardware)
    - [3.2 Software](#32-software)
    - [3.3 Netzwerkzugang](#33-netzwerkzugang)
  - [4. Infrastrukturübersicht](#4-infrastrukturübersicht)
    - [4.1 Plattform](#41-plattform)
    - [4.2 Containerlandschaft](#42-containerlandschaft)
    - [4.3 Umgebungen](#43-umgebungen)
      - [DEV](#dev)
      - [PROD](#prod)
  - [5. Docker-Netzwerkarchitektur](#5-docker-netzwerkarchitektur)
    - [5.1 EMC Hauptnetzwerk](#51-emc-hauptnetzwerk)
    - [5.2 Teilnehmer im EMC Hauptnetzwerk](#52-teilnehmer-im-emc-hauptnetzwerk)
    - [5.3 Kommunikationsmodell](#53-kommunikationsmodell)
      - [DEV](#dev-1)
      - [PROD](#prod-1)
    - [5.4 Infrastruktur-Netzwerke](#54-infrastruktur-netzwerke)
      - [mariadb\_default](#mariadb_default)
      - [uptime-kuma\_default](#uptime-kuma_default)
    - [5.5 Netzwerkgrundsätze](#55-netzwerkgrundsätze)
  - [6. Konfigurationsmanagement](#6-konfigurationsmanagement)
    - [6.1 Frontend Konfiguration](#61-frontend-konfiguration)
    - [6.2 Backend Konfiguration](#62-backend-konfiguration)
    - [6.3 Datenbank Konfiguration](#63-datenbank-konfiguration)
    - [6.4 Backup Konfiguration](#64-backup-konfiguration)
    - [6.5 Stack-Verwaltung](#65-stack-verwaltung)
  - [7. Erstinstallation](#7-erstinstallation)
    - [7.1 Installationsprinzip](#71-installationsprinzip)
    - [7.2 Docker Host vorbereiten](#72-docker-host-vorbereiten)
    - [7.3 Portainer bereitstellen](#73-portainer-bereitstellen)
    - [7.4 Netzwerke anlegen](#74-netzwerke-anlegen)
    - [7.5 Datenbank bereitstellen](#75-datenbank-bereitstellen)
    - [7.6 Backup-Service bereitstellen](#76-backup-service-bereitstellen)
    - [7.7 Backend bereitstellen](#77-backend-bereitstellen)
    - [7.8 Frontend bereitstellen](#78-frontend-bereitstellen)
    - [7.9 Monitoring bereitstellen](#79-monitoring-bereitstellen)
    - [7.10 Erstprüfung](#710-erstprüfung)
  - [8. Betriebsprozesse](#8-betriebsprozesse)
    - [8.1 Statusprüfung](#81-statusprüfung)
    - [8.2 Start von Diensten](#82-start-von-diensten)
    - [8.3 Stop von Diensten](#83-stop-von-diensten)
    - [8.4 Neustart](#84-neustart)
    - [8.5 Logprüfung](#85-logprüfung)
    - [8.6 Betriebsprüfung Frontend](#86-betriebsprüfung-frontend)
    - [8.7 Betriebsprüfung Backend](#87-betriebsprüfung-backend)
    - [8.8 Datenbankbetrieb](#88-datenbankbetrieb)
    - [8.9 Portainer Betrieb](#89-portainer-betrieb)
  - [9. Monitoring und Überwachung](#9-monitoring-und-überwachung)
    - [9.1 Überwachte Systeme](#91-überwachte-systeme)
    - [9.2 Frontend Monitoring](#92-frontend-monitoring)
    - [9.3 Backend Monitoring](#93-backend-monitoring)
    - [9.4 Infrastruktur Monitoring](#94-infrastruktur-monitoring)
    - [9.5 Benachrichtigungen](#95-benachrichtigungen)
    - [9.6 Monitoring Routine](#96-monitoring-routine)
  - [10. Backup-Konzept](#10-backup-konzept)
    - [10.1 Backup Strategie](#101-backup-strategie)
    - [10.2 Gesicherte Datenbanken](#102-gesicherte-datenbanken)
    - [10.3 Speicherort](#103-speicherort)
    - [10.4 Backup Rotation](#104-backup-rotation)
    - [10.5 Initialbackup](#105-initialbackup)
    - [10.6 Operative Backup Prüfung](#106-operative-backup-prüfung)
    - [10.7 Verantwortlichkeit](#107-verantwortlichkeit)
    - [10.8 Restore](#108-restore)

---

## 1. Zweck und Geltungsbereich

### 1.1 Ziel des Dokuments

Dieses Dokument beschreibt die Installation, den technischen Betrieb und die administrative Betreuung der EMC Mitgliederverwaltung.

Ziel ist eine nachvollziehbare technische Betriebsdokumentation für Installation, Betrieb, Wartung und Überwachung der Anwendung.

Das Dokument beschreibt den aktuell realisierten Betriebsstand einschließlich Übergangsarchitekturen.

---

### 1.2 Zielgruppe

Dieses Dokument richtet sich an technisch verantwortliche Personen.

Insbesondere:

- Systemadministration
- technische Projektverantwortliche
- Betreiber der NAS-/Docker-Infrastruktur
- zukünftige technische Nachfolger im Vereinsbetrieb

Es handelt sich nicht um ein Endanwenderhandbuch.

---

### 1.3 Abgrenzung zu anderen Dokumenten

Dieses Dokument beschreibt den technischen Betrieb.

Nicht Bestandteil:

| Thema | Dokument |
|------|----------|
| Systemarchitektur | `04-architektur.md` |
| Release / Deployment Prozesse | `02-deployment.md` |
| Benutzerbedienung | `03-benutzerhandbuch.md` |
| Fehlerbehebung / Recovery | `05-troubleshooting.md` |

---

## 2. Systemübersicht

### 2.1 Zweck des Systems

Die EMC Mitgliederverwaltung ist eine webbasierte Anwendung zur Verwaltung von Mitgliederdaten des EMC.

Ziel ist die schrittweise Ablösung der bisherigen Microsoft-Access-basierten operativen Mitgliederpflege.

Die Anwendung unterstützt aktuell:

- Mitgliederverwaltung
- Stammdatenpflege
- Kontaktdatenpflege
- Mitgliedschaftsverwaltung
- Datenschutzdaten
- Chorkleidungsverwaltung
- Benutzerverwaltung
- Rollen- und Rechteverwaltung

---

### 2.2 Betriebsmodell

Die Anwendung wird containerisiert auf einem NAS betrieben.

Betriebsmodell:

- Docker-basierter Betrieb
- Portainer-basierte Stack-Verwaltung
- getrennte DEV- und PROD-Umgebung
- zentrale MariaDB-Datenhaltung
- Monitoring via Uptime Kuma
- automatisierte Datenbank-Backups

Die Architektur befindet sich aktuell in einem produktivnahen Pilotbetrieb.

> [!NOTE]
> Die aktuelle Betriebsarchitektur enthält bewusst Übergangsstrukturen zur Microsoft-Access-Altwelt.

---

### 2.3 Systemkomponenten

Die Betriebsumgebung umfasst folgende Hauptkomponenten:

| Komponente | Funktion |
|----------|----------|
| Frontend DEV | React Webanwendung |
| Backend DEV | Spring Boot API |
| Frontend PROD | React Webanwendung |
| Backend PROD | Spring Boot API |
| MariaDB | zentrale Datenhaltung |
| mariadb-backup | automatisierte Datenbank-Backups |
| phpMyAdmin | Datenbankadministration |
| Uptime Kuma | Monitoring |
| Portainer | Container-/Stack-Verwaltung |

---

## 3. Systemvoraussetzungen

### 3.1 Hardware

Aktuelle Betriebsplattform:

```text
UGREEN DH2300 NAS
```

Erforderlich:

- Docker-fähige Hostplattform
- ausreichender Speicherplatz für Container, Images und Backups
- persistente Datenspeicherung
- stabile Netzwerkverbindung

---

### 3.2 Software

Aktuell eingesetzte Plattformsoftware:

- Docker
- Portainer
- nginx
- MariaDB
- Uptime Kuma
- phpMyAdmin

Anwendungskomponenten:

Frontend:

- React 19
- Vite
- nginx

Backend:

- Java 21
- Spring Boot 3
- Spring Security

Datenbank:

- MariaDB

---

### 3.3 Netzwerkzugang

Aktueller Zugriff:

- lokales Netzwerk
- VPN-basierter externer Zugriff

Perspektivisch geplant:

- Domain-basierter Zugriff
- Reverse Proxy
- HTTPS/TLS

Netzwerkvoraussetzungen:

- Zugriff auf NAS
- Docker Netzwerkkommunikation
- Zugriff auf veröffentlichte Frontend-Ports

---

## 4. Infrastrukturübersicht

### 4.1 Plattform

Die Anwendung läuft vollständig auf einer zentralen NAS-Plattform.

Aktuelle Plattform:

```text
UGREEN DH2300
```

Betriebsmodell:

- zentraler Container-Host
- Docker Runtime
- Stack-Verwaltung via Portainer

Die Anwendung ist nicht auf mehrere Hosts verteilt.

---

### 4.2 Containerlandschaft

Aktuell laufende Container:

| Container | Funktion |
|----------|----------|
| emc-mitglieder-frontend-dev | DEV Frontend |
| emc-mitglieder-backend-dev | DEV Backend |
| emc-mitglieder-frontend-prod | PROD Frontend |
| emc-mitglieder-backend-prod | PROD Backend |
| mariadb | zentrale Datenbank |
| mariadb-backup | Backup-Service |
| phpmyadmin | Datenbankadministration |
| uptime-kuma | Monitoring |
| portainer | Containerverwaltung |

---

### 4.3 Umgebungen

Die Anwendung unterscheidet zwei technische Umgebungen.

#### DEV

Zweck:

- Entwicklung
- Tests
- technische Validierung
- Vorstufe für PROD

Komponenten:

- DEV Frontend
- DEV Backend
- DEV Datenbank

---

#### PROD

Zweck:

- produktivnaher Pilotbetrieb
- operative Nutzung

Komponenten:

- PROD Frontend
- PROD Backend
- PROD Datenbank

> [!NOTE]
> DEV und PROD teilen sich aktuell dieselbe MariaDB-Instanz, verwenden jedoch getrennte Datenbanken.

---

## 5. Docker-Netzwerkarchitektur

Die EMC Mitgliederverwaltung verwendet mehrere Docker-Netzwerke mit klarer funktionaler Trennung.

---

### 5.1 EMC Hauptnetzwerk

Zentrales Betriebsnetzwerk:

```text
emc_net
```

Eigenschaften:

- Docker Bridge Netzwerk
- internes Anwendungsnetz
- Kommunikation zwischen EMC-Anwendungskomponenten

Technische Eigenschaften:

```text
Subnetz: 172.18.0.0/16
Typ: bridge
```

---

### 5.2 Teilnehmer im EMC Hauptnetzwerk

Aktuell angeschlossene Container:

- emc-mitglieder-frontend-dev
- emc-mitglieder-backend-dev
- emc-mitglieder-frontend-prod
- emc-mitglieder-backend-prod
- mariadb
- uptime-kuma

Dieses Netzwerk bildet das zentrale Kommunikationsnetz der EMC Anwendung.

---

### 5.3 Kommunikationsmodell

#### DEV

Kommunikationsfluss:

```mermaid
graph LR
    Browser --> FrontendDEV["Frontend DEV"]
    FrontendDEV --> BackendDEV["Backend DEV"]
    BackendDEV --> MariaDB["MariaDB"]
```

---

#### PROD

Kommunikationsfluss:

```mermaid
graph LR
    Browser --> FrontendPROD["Frontend PROD"]
    FrontendPROD --> BackendPROD["Backend PROD"]
    BackendPROD --> MariaDB["MariaDB"]
```

---

### 5.4 Infrastruktur-Netzwerke

Zusätzlich existieren weitere Docker-Netzwerke.

#### mariadb_default

Zweck:

Datenbanknahe Infrastruktur

Teilnehmer:

- mariadb
- phpmyadmin
- mariadb-backup

Dieses Netzwerk dient Datenbankadministration und Backup.

---

#### uptime-kuma_default

Zweck:

Monitoring-Infrastruktur

Teilnehmer:

- uptime-kuma

---

### 5.5 Netzwerkgrundsätze

Architekturprinzipien:

- Backend nicht direkt extern exponieren
- interne Containerkommunikation über Docker-Netzwerke
- funktionale Netztrennung
- Monitoring separat
- Datenbankinfrastruktur separat

---

## 6. Konfigurationsmanagement

Die Anwendung nutzt komponentenspezifische Konfiguration.

Die Konfiguration ist abhängig von Umgebung und Komponente.

---

### 6.1 Frontend Konfiguration

Das Frontend wird als statischer React Build über nginx ausgeliefert.

Konfigurationsbestandteile:

- nginx Konfiguration
- API Reverse Proxy Routing
- React Build Artefakte

Aktuelle Proxy-Konfiguration:

```text
location /api/
proxy_pass -> Backend
```

DEV:

```text
emc-mitglieder-backend-dev:8080
```

PROD:

```text
emc-mitglieder-backend-prod:8080
```

Zweck:

- gleiche Browser-Origin
- Backend nicht direkt veröffentlichen
- vereinfachte Sessionkommunikation

---

### 6.2 Backend Konfiguration

Das Backend wird containerisiert betrieben.

Typische Konfigurationsbereiche:

- Datenbankverbindung
- Spring Profile
- Sicherheitskonfiguration
- Session-Verhalten
- Logging

Umgebungsspezifisch:

- DEV
- PROD

---

### 6.3 Datenbank Konfiguration

MariaDB dient als zentrale Datenhaltung.

Aktuelle Datenbanken:

| Umgebung | Datenbank |
|---------|-----------|
| DEV | emc_mitglieder_dev |
| PROD | emc_mitglieder |

Zusätzlich vorhanden:

- emc_finanzen
- emc_finanzen_dev

---

### 6.4 Backup Konfiguration

Automatisierte Datenbank-Backups erfolgen über:

```text
fradelg/mysql-cron-backup
```

Container:

```text
mariadb-backup
```

Aktuelle Konfiguration:

| Parameter | Wert |
|---------|------|
| Backup Zeit | täglich 03:00 |
| Initial Backup | aktiviert |
| Retention | 14 Backups |
| Kompression | gzip Level 6 |

Gesicherte Datenbanken:

- emc_mitglieder
- emc_mitglieder_dev
- emc_finanzen
- emc_finanzen_dev

Speicherort:

```text
/volume1/home/JaitiNissi1968/docker/backups/mariadb
```

---

### 6.5 Stack-Verwaltung

Die technische Betriebsstrategie basiert auf:

```text
Portainer Stacks
```

Stacks dienen zur Verwaltung von:

- Frontend DEV
- Backend DEV
- Frontend PROD
- Backend PROD
- Infrastrukturservices

Docker CLI bleibt als Diagnose- und Recovery-Werkzeug verfügbar.

---

## 7. Erstinstallation

Dieses Kapitel beschreibt die grundsätzliche technische Erstinstallation.

Release- und Update-Prozesse werden separat im Deployment-Handbuch beschrieben.

---

### 7.1 Installationsprinzip

Installationsreihenfolge:

1. Docker Host bereitstellen
2. Portainer bereitstellen
3. Docker Netzwerke anlegen
4. MariaDB bereitstellen
5. Backup-Service bereitstellen
6. Backend Stacks bereitstellen
7. Frontend Stacks bereitstellen
8. Monitoring bereitstellen
9. Funktionsprüfung durchführen

---

### 7.2 Docker Host vorbereiten

Erforderlich:

- betriebsbereiter Docker Host
- persistente Speicherbereiche
- Netzwerkzugriff
- Docker Runtime

Aktuelle Plattform:

```text
UGREEN DH2300
```

---

### 7.3 Portainer bereitstellen

Portainer dient als zentrale Verwaltungsoberfläche.

Funktion:

- Stack Deployment
- Containerverwaltung
- Logs
- Neustarts
- Statuskontrolle

Aktueller Zugriff:

```text
Port 9000
```

---

### 7.4 Netzwerke anlegen

Erforderliche Netzwerke:

```text
emc_net
mariadb_default
uptime-kuma_default
```

Je nach Installationsstrategie können Netzwerke automatisch oder manuell erzeugt werden.

---

### 7.5 Datenbank bereitstellen

MariaDB bereitstellen mit:

- persistentem Volume
- interner Netzwerkkommunikation
- Portfreigabe für interne Altprozesse

Aktuell bewusst veröffentlicht:

```text
3306
```

Grund:

Microsoft Access / ODBC Übergangsarchitektur

> [!WARNING]
> Port 3306 darf nicht öffentlich ins Internet veröffentlicht werden.

---

### 7.6 Backup-Service bereitstellen

Backup-Container bereitstellen:

```text
mariadb-backup
```

Anforderungen:

- Zugriff auf MariaDB
- persistenter Backup-Speicher
- Cron Scheduling
- Aufbewahrungsstrategie

Funktionsprüfung:

- Initialbackup vorhanden
- Backupdateien erzeugt
- Rotation funktioniert

---

### 7.7 Backend bereitstellen

Backend-Deployment:

- DEV Stack
- PROD Stack

Anforderungen:

- Datenbankzugriff
- interne Netzwerkkommunikation
- korrekte Umgebungsparameter

Backend wird nicht direkt extern veröffentlicht.

---

### 7.8 Frontend bereitstellen

Frontend-Deployment:

- DEV Frontend
- PROD Frontend

Anforderungen:

- nginx Konfiguration
- API Proxy
- React Build Artefakte

Aktuelle veröffentlichte Ports:

| Umgebung | Port |
|---------|------|
| DEV | 8082 |
| PROD | 9082 |

---

### 7.9 Monitoring bereitstellen

Monitoring via:

```text
Uptime Kuma
```

Einzurichten:

- Frontend Checks
- Backend Checks
- Datenbank Checks
- Infrastruktur Checks
- Telegram Benachrichtigung

---

### 7.10 Erstprüfung

Nach Installation prüfen:

- alle Container laufen
- Portainer erreichbar
- Frontends erreichbar
- Login funktioniert
- Backend erreichbar
- Datenbank erreichbar
- Monitoring aktiv
- Backup erzeugt

> [!NOTE]
> Release Smoke Tests werden im Deployment-Handbuch detailliert beschrieben.

---

## 8. Betriebsprozesse

Dieses Kapitel beschreibt typische operative Betriebsabläufe.

Die primäre Verwaltungsoberfläche ist:

```text
Portainer
```

Docker CLI dient ergänzend für Diagnose und Recovery.

---

### 8.1 Statusprüfung

Regelmäßige Statuskontrolle umfasst:

- Containerstatus
- Erreichbarkeit der Webanwendung
- Monitoring Status
- Backup Status

Portainer:

- Stack Status
- Container Status
- Neustarts
- Logs

Docker CLI:

```bash
docker ps
```

Zweck:

- laufende Container prüfen
- Portfreigaben prüfen
- Neustarts erkennen

---

### 8.2 Start von Diensten

Im Regelbetrieb starten Container automatisch.

Konfiguration:

```text
restart: unless-stopped
```

Manueller Start:

Portainer:

- Stack starten
- Container starten

Docker CLI:

```bash
docker start <container>
```

oder:

```bash
docker compose up -d
```

---

### 8.3 Stop von Diensten

Geplanter Stop:

Portainer:

- Stack stoppen
- Container stoppen

Docker CLI:

```bash
docker stop <container>
```

oder:

```bash
docker compose down
```

---

### 8.4 Neustart

Typische Anwendungsfälle:

- Konfigurationsänderungen
- Deployment
- Recovery
- Netzwerkprobleme
- Containerfehler

Portainer:

- Restart Container
- Redeploy Stack

Docker CLI:

```bash
docker restart <container>
```

---

### 8.5 Logprüfung

Logs sind zentrale Diagnosequelle.

Portainer:

integrierte Logansicht

Docker CLI:

```bash
docker logs <container>
```

Live Log:

```bash
docker logs -f <container>
```

Typische Kandidaten:

- emc-mitglieder-backend-dev
- emc-mitglieder-backend-prod
- emc-mitglieder-frontend-dev
- emc-mitglieder-frontend-prod
- mariadb
- mariadb-backup
- uptime-kuma

---

### 8.6 Betriebsprüfung Frontend

Prüfen:

- Frontend erreichbar
- Loginseite lädt
- Navigation funktioniert
- Sessionverhalten korrekt

Aktuelle Zugriffe:

DEV:

```text
http://<NAS-IP>:8082
```

PROD:

```text
http://<NAS-IP>:9082
```

---

### 8.7 Betriebsprüfung Backend

Backend ist nicht direkt extern veröffentlicht.

Prüfung erfolgt indirekt:

- Frontend Funktion
- Uptime Kuma
- Backend Logs

Bewusst erwartetes Verhalten:

```text
HTTP 401 Unauthorized
```

ohne Authentifizierung.

---

### 8.8 Datenbankbetrieb

Regelmäßig prüfen:

- MariaDB Container läuft
- Portfreigabe vorhanden
- DB Verbindungen stabil
- keine Fehlereinträge

Port:

```text
3306
```

---

### 8.9 Portainer Betrieb

Portainer dient als zentrale Betriebsoberfläche.

Funktionen:

- Stack Verwaltung
- Container Status
- Logs
- Neustarts
- Redeployments

Aktueller Zugriff:

```text
http://<NAS-IP>:9000
```

---

## 9. Monitoring und Überwachung

Monitoring erfolgt über:

```text
Uptime Kuma
```

---

### 9.1 Überwachte Systeme

Aktuell überwacht:

- Frontend DEV
- Frontend PROD
- Backend DEV
- Backend PROD
- MariaDB
- phpMyAdmin
- Portainer
- mariadb-backup

---

### 9.2 Frontend Monitoring

Frontend Checks prüfen:

- HTTP Erreichbarkeit
- nginx Funktion
- Webanwendung erreichbar

Erwartung:

```text
HTTP 200
```

---

### 9.3 Backend Monitoring

Backend Monitoring erfolgt bewusst über geschützte Endpunkte.

Erwartetes Ergebnis:

```text
HTTP 401 Unauthorized
```

Dies gilt als erfolgreiches Monitoring-Ergebnis.

Interpretation:

- Backend erreichbar
- Spring Security aktiv
- API reagiert erwartungsgemäß

> [!NOTE]
> HTTP 401 ist hier kein Fehlerzustand.

---

### 9.4 Infrastruktur Monitoring

Überwachung:

- MariaDB
- phpMyAdmin
- Portainer
- Backup Container

Ziele:

- Erreichbarkeit
- Dienststatus
- Ausfälle erkennen

---

### 9.5 Benachrichtigungen

Benachrichtigung erfolgt via:

```text
Telegram
```

Zweck:

- schnelle Ausfallinformation
- Betriebsüberwachung
- Alarmierung

---

### 9.6 Monitoring Routine

Empfohlene regelmäßige Prüfung:

- Uptime Kuma Dashboard
- Fehlgeschlagene Checks
- Neustarts
- Telegram Alerts
- Erreichbarkeit Frontend

---

## 10. Backup-Konzept

Die Anwendung verwendet automatisierte Datenbank-Backups.

Technologie:

```text
fradelg/mysql-cron-backup
```

Container:

```text
mariadb-backup
```

---

### 10.1 Backup Strategie

Backup-Modell:

- automatisiert
- dateibasiert
- containerisiert
- komprimiert
- rotationsbasiert

Aktuelle Parameter:

| Parameter | Wert |
|---------|------|
| Zeitplan | täglich 03:00 |
| Initialbackup | aktiviert |
| Retention | 14 Backups |
| Kompression | gzip Level 6 |

---

### 10.2 Gesicherte Datenbanken

Aktuell enthalten:

- emc_mitglieder
- emc_mitglieder_dev
- emc_finanzen
- emc_finanzen_dev

> [!NOTE]
> Das Backup-Konzept umfasst aktuell mehrere EMC-bezogene Anwendungen, nicht ausschließlich die Mitgliederverwaltung.

---

### 10.3 Speicherort

Aktueller Backup Speicher:

```text
/volume1/home/JaitiNissi1968/docker/backups/mariadb
```

Anforderungen:

- persistenter Speicher
- ausreichender freier Platz
- NAS-seitige Datensicherheit

---

### 10.4 Backup Rotation

Retention:

```text
14 Backups
```

Bedeutung:

- ältere Backups werden automatisch entfernt
- begrenzter Speicherverbrauch
- definierte Aufbewahrung

---

### 10.5 Initialbackup

Beim Start des Backup Containers:

```text
INIT_BACKUP=1
```

Bedeutung:

Sofortiger Initialdump beim Containerstart.

Vorteil:

Backup-Sicherheit direkt nach Deployment oder Neustart.

---

### 10.6 Operative Backup Prüfung

Regelmäßig prüfen:

- Backupdateien vorhanden
- aktuelles Datum vorhanden
- Dateigröße plausibel
- Rotation funktioniert
- Backup Container läuft

Prüfung via:

Portainer:

- Container Status
- Logs

Docker CLI:

```bash
docker logs mariadb-backup
```

NAS Dateisystem:

Backup Verzeichnis prüfen

---

### 10.7 Verantwortlichkeit

Der technische Betreiber ist verantwortlich für:

- Backup Funktionsprüfung
- Speicherplatzkontrolle
- Rotation Kontrolle
- Backup Integrität

---

### 10.8 Restore

Restore-Prozesse werden detailliert im Troubleshooting-/Recovery-Handbuch beschrieben.

> [!WARNING]
> Ein Backup ohne getesteten Restore ist kein vollständiges Betriebskonzept.

---

