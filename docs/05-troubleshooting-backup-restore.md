# Troubleshooting / Backup / Restore

## Inhaltsverzeichnis

- [Troubleshooting / Backup / Restore](#troubleshooting--backup--restore)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [1. Zweck und Geltungsbereich](#1-zweck-und-geltungsbereich)
    - [1.1 Abgrenzung](#11-abgrenzung)
    - [1.2 Zielgruppe](#12-zielgruppe)
  - [2. Betriebsgrundsätze](#2-betriebsgrundsätze)
    - [2.1 Erst beobachten, dann handeln](#21-erst-beobachten-dann-handeln)
    - [2.2 Keine Blindmaßnahmen](#22-keine-blindmaßnahmen)
    - [2.3 Docker Realität prüfen](#23-docker-realität-prüfen)
    - [2.4 Produktive Daten schützen](#24-produktive-daten-schützen)
    - [2.5 Änderungen dokumentieren](#25-änderungen-dokumentieren)
  - [3. Monitoring und Erstdiagnose](#3-monitoring-und-erstdiagnose)
    - [3.1 Uptime Kuma Monitoring](#31-uptime-kuma-monitoring)
    - [3.2 Telegram Benachrichtigungen](#32-telegram-benachrichtigungen)
    - [3.3 Erstdiagnose Ablauf](#33-erstdiagnose-ablauf)
    - [3.4 Telegram Fehlerdiagnose](#34-telegram-fehlerdiagnose)
  - [4. Standard Diagnosekommandos](#4-standard-diagnosekommandos)
    - [4.1 Container Status](#41-container-status)
    - [4.2 Logs](#42-logs)
    - [4.3 Container Konfiguration](#43-container-konfiguration)
    - [4.4 Docker Netzwerke](#44-docker-netzwerke)
    - [4.5 Docker Images](#45-docker-images)
    - [4.6 Speicherplatz](#46-speicherplatz)
    - [4.7 Arbeitsspeicher](#47-arbeitsspeicher)
    - [4.8 CPU / Prozesse](#48-cpu--prozesse)
    - [4.9 Docker Mounts prüfen](#49-docker-mounts-prüfen)
    - [4.10 Build-Verzeichnisse prüfen](#410-build-verzeichnisse-prüfen)
    - [4.11 Rechteprobleme](#411-rechteprobleme)
  - [5. Typische Fehlerbilder](#5-typische-fehlerbilder)
    - [5.1 Frontend lädt nicht](#51-frontend-lädt-nicht)
    - [5.2 Login funktioniert nicht](#52-login-funktioniert-nicht)
    - [5.3 Backend startet nicht](#53-backend-startet-nicht)
    - [5.4 Datenbankverbindung schlägt fehl](#54-datenbankverbindung-schlägt-fehl)
    - [5.5 Auto-Save funktioniert nicht](#55-auto-save-funktioniert-nicht)
    - [5.6 Permission denied](#56-permission-denied)
    - [5.7 Monitoring funktioniert nicht](#57-monitoring-funktioniert-nicht)
    - [5.8 Externer Zugriff nicht möglich](#58-externer-zugriff-nicht-möglich)
  - [6. Backup-Strategie](#6-backup-strategie)
    - [6.1 Standard Backup](#61-standard-backup)
    - [6.2 Backup Ziel](#62-backup-ziel)
    - [6.3 Backup Grundregeln](#63-backup-grundregeln)
  - [7. Manuelle Backups](#7-manuelle-backups)
    - [7.1 DEV Datenbank Backup](#71-dev-datenbank-backup)
    - [7.2 PROD Datenbank Backup](#72-prod-datenbank-backup)
    - [7.3 Backup prüfen](#73-backup-prüfen)
    - [7.4 Backup Benennung](#74-backup-benennung)
    - [7.5 Anwendungsfälle für manuelle Backups](#75-anwendungsfälle-für-manuelle-backups)
  - [8. Restore Grundprinzipien](#8-restore-grundprinzipien)
    - [8.1 Restore Grundregeln](#81-restore-grundregeln)
    - [8.2 Restore Warnung](#82-restore-warnung)
    - [8.3 Restore Entscheidungsregel](#83-restore-entscheidungsregel)
  - [9. Restore Verfahren](#9-restore-verfahren)
    - [9.1 DEV Restore](#91-dev-restore)
    - [9.2 PROD Restore](#92-prod-restore)
    - [9.3 Restore automatisierter Backups](#93-restore-automatisierter-backups)
    - [9.4 Restore Nachkontrolle](#94-restore-nachkontrolle)
    - [9.5 Restore Sicherheitsregeln](#95-restore-sicherheitsregeln)
  - [10. Deployment Recovery](#10-deployment-recovery)
    - [10.1 Frontend Recovery](#101-frontend-recovery)
    - [10.2 Backend Recovery](#102-backend-recovery)
    - [10.3 Monitoring Recovery](#103-monitoring-recovery)
  - [11. Datenbank Notfallmaßnahmen](#11-datenbank-notfallmaßnahmen)
    - [11.1 Sofortmaßnahmen](#111-sofortmaßnahmen)
    - [11.2 MariaDB prüfen](#112-mariadb-prüfen)
    - [11.3 Datenbankplatz prüfen](#113-datenbankplatz-prüfen)
    - [11.4 Datenbankintegrität prüfen](#114-datenbankintegrität-prüfen)
    - [11.5 Backup Infrastruktur prüfen](#115-backup-infrastruktur-prüfen)
    - [11.6 Access Übergangssystem berücksichtigen](#116-access-übergangssystem-berücksichtigen)
    - [11.7 Eskalationsregel](#117-eskalationsregel)
  - [12. Recovery Checkliste](#12-recovery-checkliste)
    - [12.1 Erstdiagnose](#121-erstdiagnose)
    - [12.2 Technische Prüfung](#122-technische-prüfung)
    - [12.3 Entscheidung](#123-entscheidung)
    - [12.4 Nach Recovery](#124-nach-recovery)

---

## 1. Zweck und Geltungsbereich

Dieses Dokument beschreibt standardisierte Verfahren für:

- Störungsdiagnose
- Fehleranalyse
- Monitoring
- Backup
- Restore
- Recovery

für die EMC Mitgliederverwaltung.

Ziel ist ein reproduzierbares technisches Runbook für den operativen Betrieb.

---

### 1.1 Abgrenzung

Nicht Bestandteil dieses Dokuments:

| Thema | Dokument |
|------|----------|
| Installation / Betrieb | `01-installation-betrieb.md` |
| Deployment | `02-deployment.md` |
| Benutzerbedienung | `03-benutzerhandbuch.md` |
| Architektur | `04-architektur.md` |

---

### 1.2 Zielgruppe

Dieses Dokument richtet sich an:

- technischen Betreiber
- Administratoren
- Entwickler
- technische Projektverantwortliche

---

## 2. Betriebsgrundsätze

Im Störungsfall gelten folgende Grundprinzipien.

---

### 2.1 Erst beobachten, dann handeln

Nicht sofort Maßnahmen durchführen.

Zuerst prüfen:

- Was ist betroffen?
- Seit wann?
- Welche Änderung war zuletzt?
- Ist DEV oder PROD betroffen?
- Ist nur ein Dienst oder mehrere betroffen?

---

### 2.2 Keine Blindmaßnahmen

Nicht ohne Diagnose:

- Container löschen
- Images löschen
- Datenbankeingriffe
- Docker Netzwerke entfernen
- Verzeichnisse leeren

---

### 2.3 Docker Realität prüfen

NAS GUI kann irreführend sein.

Verbindlich sind reale Docker Zustände.

Prüfen mit:

```bash
docker ps
docker inspect <container>
docker network ls
```

---

### 2.4 Produktive Daten schützen

Bei PROD besonders vorsichtig.

Niemals unüberlegt:

- Tabellen löschen
- Daten ändern
- Restore starten
- Datenbank überschreiben

---

### 2.5 Änderungen dokumentieren

Bei Eingriffen dokumentieren:

- Zeitpunkt
- Maßnahme
- Ursache
- Ergebnis

---

## 3. Monitoring und Erstdiagnose

Monitoring ist der erste Einstieg in die Fehleranalyse.

---

### 3.1 Uptime Kuma Monitoring

Überwachte Dienste:

- Frontend DEV
- Frontend PROD
- Backend DEV
- Backend PROD
- MariaDB
- phpMyAdmin
- Portainer
- Backup Infrastruktur

---

### 3.2 Telegram Benachrichtigungen

Telegram Alerts sind aktiv und getestet.

Verifiziert:

- DOWN Alerts funktionieren
- UP Alerts funktionieren

Typischer Alert-Test:

```bash
docker stop emc-mitglieder-backend-dev
docker start emc-mitglieder-backend-dev
```

---

### 3.3 Erstdiagnose Ablauf

Empfohlene Reihenfolge:

1. Uptime Kuma prüfen
2. Telegram letzte Alerts prüfen
3. betroffenen Dienst identifizieren
4. Container Status prüfen
5. Logs prüfen
6. Netzwerk prüfen
7. Speicher / Ressourcen prüfen

---

### 3.4 Telegram Fehlerdiagnose

Typisches Symptom:

```text
404 Not Found
```

Telegram API Test:

```bash
curl "https://api.telegram.org/bot<TOKEN>/getMe"
```

Testnachricht:

```bash
curl -X POST "https://api.telegram.org/bot<TOKEN>/sendMessage" \
  -d chat_id=<CHAT_ID> \
  -d text="EMC Monitoring Test"
```

Typische Ursachen:

- ungültiger Bot Token
- falsche Chat-ID
- fehlerhafte Uptime Kuma Konfiguration
- fehlender Internetzugang

---

## 4. Standard Diagnosekommandos

Diese Kommandos bilden das technische Standardwerkzeug.

---

### 4.1 Container Status

Laufende Container:

```bash
docker ps
```

Alle Container:

```bash
docker ps -a
```

Gezielt EMC:

```bash
docker ps | grep emc
```

---

### 4.2 Logs

Backend DEV:

```bash
docker logs emc-mitglieder-backend-dev --tail 100
```

Backend PROD:

```bash
docker logs emc-mitglieder-backend-prod --tail 100
```

Frontend DEV:

```bash
docker logs emc-mitglieder-frontend-dev --tail 100
```

Frontend PROD:

```bash
docker logs emc-mitglieder-frontend-prod --tail 100
```

MariaDB:

```bash
docker logs mariadb --tail 100
```

Uptime Kuma:

```bash
docker logs uptime-kuma --tail 100
```

---

### 4.3 Container Konfiguration

Backend DEV:

```bash
docker inspect emc-mitglieder-backend-dev
```

Backend PROD:

```bash
docker inspect emc-mitglieder-backend-prod
```

Frontend DEV:

```bash
docker inspect emc-mitglieder-frontend-dev
```

Frontend PROD:

```bash
docker inspect emc-mitglieder-frontend-prod
```

---

### 4.4 Docker Netzwerke

Alle Netzwerke:

```bash
docker network ls
```

Netzwerk Details:

```bash
docker network inspect emc-net
```

MariaDB Netzwerk:

```bash
docker network inspect mariadb_default
```

---

### 4.5 Docker Images

Images prüfen:

```bash
docker image ls
```

Backend Images:

```bash
docker image ls | grep emc-mitglieder-backend
```

---

### 4.6 Speicherplatz

Dateisystem:

```bash
df -h
```

Docker Speicher:

```bash
docker system df
```

---

### 4.7 Arbeitsspeicher

RAM:

```bash
free -h
```

---

### 4.8 CPU / Prozesse

Prozesse:

```bash
top
```

---

### 4.9 Docker Mounts prüfen

Frontend DEV:

```bash
docker inspect emc-mitglieder-frontend-dev | grep -A 20 Mounts
```

Frontend PROD:

```bash
docker inspect emc-mitglieder-frontend-prod | grep -A 20 Mounts
```

---

### 4.10 Build-Verzeichnisse prüfen

Build Struktur:

```bash
ls -la /volume1/docker/build
```

Frontend DEV:

```bash
ls -la /volume1/docker/build/mitglieder-frontend-dev
```

Frontend PROD:

```bash
ls -la /volume1/docker/build/mitglieder-frontend-prod
```

Backend DEV:

```bash
find /volume1/docker/build/mitglieder-backend-dev -maxdepth 2 -type f -print
```

Backend PROD:

```bash
find /volume1/docker/build/mitglieder-backend-prod -maxdepth 2 -type f -print
```

---

### 4.11 Rechteprobleme

Typischer Fehler:

```text
Permission denied
```

Prüfen:

```bash
ls -la /volume1/docker/build
```

Typischer Fix:

```bash
sudo chown -R JaitiNissi1968:users /volume1/docker/build
```

---

## 5. Typische Fehlerbilder

Dieses Kapitel beschreibt bekannte praktische Fehlerbilder und empfohlene Diagnosewege.

---

### 5.1 Frontend lädt nicht

Typische Symptome:

- Browser zeigt leere Seite
- nginx Fehlerseite
- Assets fehlen
- Anwendung startet nicht

Prüfen:

Container läuft?

```bash
docker ps | grep emc-mitglieder-frontend
```

Logs:

```bash
docker logs emc-mitglieder-frontend-dev --tail 100
docker logs emc-mitglieder-frontend-prod --tail 100
```

Mounts:

```bash
docker inspect emc-mitglieder-frontend-dev | grep -A 20 Mounts
docker inspect emc-mitglieder-frontend-prod | grep -A 20 Mounts
```

Build-Dateien:

```bash
ls -la /volume1/docker/build/mitglieder-frontend-dev/dist
ls -la /volume1/docker/build/mitglieder-frontend-prod/dist
```

Typische Ursachen:

- dist fehlt
- dist unvollständig
- nginx.conf falsch
- falscher Redeploy
- Browser Cache

---

### 5.2 Login funktioniert nicht

Typische Symptome:

- Login schlägt fehl
- Session verschwindet
- API Fehler
- 401 / 403

Prüfen:

Backend läuft?

```bash
docker ps | grep emc-mitglieder-backend
```

Logs:

```bash
docker logs emc-mitglieder-backend-dev --tail 100
docker logs emc-mitglieder-backend-prod --tail 100
```

MariaDB erreichbar?

```bash
docker ps | grep mariadb
```

Typische Ursachen:

- Backend down
- Datenbank down
- Sessionproblem
- Auth Konfigurationsproblem

---

### 5.3 Backend startet nicht

Typische Symptome:

- Container startet nicht
- Restart Loop
- Spring Boot Fehler

Prüfen:

```bash
docker ps -a | grep emc-mitglieder-backend
```

Logs:

```bash
docker logs emc-mitglieder-backend-dev --tail 150
docker logs emc-mitglieder-backend-prod --tail 150
```

Artefakte:

```bash
find /volume1/docker/build/mitglieder-backend-dev -maxdepth 2 -type f -print
find /volume1/docker/build/mitglieder-backend-prod -maxdepth 2 -type f -print
```

Dockerfile:

```bash
cat /volume1/docker/build/mitglieder-backend-dev/Dockerfile
cat /volume1/docker/build/mitglieder-backend-prod/Dockerfile
```

Typische Ursachen:

- JAR fehlt
- falsche JAR
- Dockerfile fehlerhaft
- Spring Konfigurationsproblem
- ENV Problem

---

### 5.4 Datenbankverbindung schlägt fehl

Typische Symptome:

- Backend startet nicht
- SQL Exceptions
- Login unmöglich

Prüfen:

MariaDB läuft?

```bash
docker ps | grep mariadb
```

Logs:

```bash
docker logs mariadb --tail 150
```

Backend ENV prüfen:

```bash
docker inspect emc-mitglieder-backend-dev
docker inspect emc-mitglieder-backend-prod
```

Netzwerke:

```bash
docker network ls
docker network inspect emc-net
docker network inspect mariadb_default
```

Typische Ursachen:

- MariaDB down
- falsche Credentials
- Netzwerkproblem
- DB nicht erreichbar

---

### 5.5 Auto-Save funktioniert nicht

Typische Symptome:

- Speichern hängt
- Fehleranzeige im Frontend
- Änderungen verschwinden

Prüfen:

Backend Logs:

```bash
docker logs emc-mitglieder-backend-dev --tail 100
docker logs emc-mitglieder-backend-prod --tail 100
```

Frontend Browser Console prüfen.

Typische Ursachen:

- Backend Fehler
- Session abgelaufen
- Validation Fehler
- Datenbankproblem

---

### 5.6 Permission denied

Typische Symptome:

```text
Permission denied
```

Prüfen:

```bash
ls -la /volume1/docker/build
```

Typischer Fix:

```bash
sudo chown -R JaitiNissi1968:users /volume1/docker/build
```

---

### 5.7 Monitoring funktioniert nicht

Typische Symptome:

- keine Alerts
- Telegram Fehler
- falscher Monitorstatus

Telegram API Test:

```bash
curl "https://api.telegram.org/bot<TOKEN>/getMe"
```

Telegram Nachricht:

```bash
curl -X POST "https://api.telegram.org/bot<TOKEN>/sendMessage" \
  -d chat_id=<CHAT_ID> \
  -d text="EMC Monitoring Test"
```

Uptime Logs:

```bash
docker logs uptime-kuma --tail 100
```

---

### 5.8 Externer Zugriff nicht möglich

Typische Symptome:

- Webanwendung nicht erreichbar
- Portainer nicht erreichbar
- phpMyAdmin nicht erreichbar
- NAS Weboberfläche nicht erreichbar

Prüfen:

Ist WireGuard VPN aktiv?

Client prüfen:

- WireGuard Verbindung aktiv?
- Tunnel verbunden?
- korrekte Fritz!Box Verbindung?

Typische Ursachen:

- VPN Tunnel nicht aktiv
- VPN Verbindung getrennt
- Fritz!Box VPN Problem
- lokales Netzwerkproblem
- Internetverbindung gestört

Abgrenzung:

Wenn Dienste intern auf dem NAS laufen, aber extern nicht erreichbar sind, liegt die Ursache häufig nicht bei Docker oder der Anwendung, sondern beim VPN-Zugriff.

---

## 6. Backup-Strategie

Die EMC Mitgliederverwaltung nutzt eine mehrstufige Backup-Strategie.

---

### 6.1 Standard Backup

Aktuelle Backup-Infrastruktur:

automatisierter Docker Container:

```text
mariadb-backup
```

Aktuelles Verhalten:

- tägliche automatisierte Datenbank-Dumps
- komprimierte Sicherungen (`.sql.gz`)
- Sicherung mehrerer Datenbanken
- zeitgestempelte Sicherungen
- zusätzlich aktuelle `latest.*` Referenzdateien

Beispiele:

```text
202605220300.emc_mitglieder.sql.gz
202605220300.emc_mitglieder_dev.sql.gz
latest.emc_mitglieder.sql.gz
latest.emc_mitglieder_dev.sql.gz
```

---

### 6.2 Backup Ziel

Aktueller Backup-Pfad:

```text
/volume1/home/JaitiNissi1968/docker/backups/mariadb
```

Container prüfen:

```bash
docker ps | grep mariadb-backup
```

Mount prüfen:

```bash
docker inspect mariadb-backup | grep -A 20 Mounts
```

Hinweis:

Dieser Pfad ist historisch gewachsen.

Eine spätere Standardisierung auf:

```text
/volume1/docker/backups/mariadb
```

ist vorgesehen.

---

### 6.3 Backup Grundregeln

Vor kritischen Änderungen:

zusätzlich manuelles Backup.

Insbesondere vor:

- Datenbankänderungen
- Schemaänderungen
- größeren Releases
- Restore Maßnahmen

---

## 7. Manuelle Backups

Dieses Kapitel beschreibt zusätzliche manuelle Sicherungen.

Hinweis:

Manuelle Backups sind Zusatzsicherungen.

Der reguläre tägliche Schutz erfolgt über den automatisierten `mariadb-backup` Container.

---

### 7.1 DEV Datenbank Backup

Backup erzeugen:

```bash
docker exec mariadb mariadb-dump -u root -p emc_mitglieder_dev > emc_mitglieder_dev_backup.sql
```

---

### 7.2 PROD Datenbank Backup

Backup erzeugen:

```bash
docker exec mariadb mariadb-dump -u root -p emc_mitglieder > emc_mitglieder_prod_backup.sql
```

---

### 7.3 Backup prüfen

Datei prüfen:

```bash
ls -lh *.sql
```

Inhalt prüfen:

```bash
head -20 emc_mitglieder_prod_backup.sql
```

---

### 7.4 Backup Benennung

Empfohlen:

```text
emc_mitglieder_prod_YYYY-MM-DD.sql
```

Beispiel:

```text
emc_mitglieder_prod_2026-05-22.sql
```

---

### 7.5 Anwendungsfälle für manuelle Backups

Manuelle Zusatzsicherungen sind insbesondere sinnvoll vor:

- Datenbank-Schemaänderungen
- größeren Releases
- PROD Migrationen
- Restore-Maßnahmen
- technischen Bereinigungen
- riskanten administrativen Eingriffen

Beispiele:

- PROD Migration Phase 3c
- Datenmodelländerungen
- strukturelle Refactorings

---

## 8. Restore Grundprinzipien

Restore ist kritisch.

---

### 8.1 Restore Grundregeln

Vor Restore prüfen:

- DEV oder PROD?
- welche Datenbank?
- aktuelles Backup vorhanden?
- Ziel wirklich korrekt?

---

### 8.2 Restore Warnung

> [!WARNING]
> Restore kann produktive Daten überschreiben oder zerstören.

Restore niemals unüberlegt durchführen.

---

### 8.3 Restore Entscheidungsregel

Restore nur wenn:

- Ursache verstanden
- Backup geprüft
- Auswirkungen verstanden
- produktive Risiken bewertet

---

## 9. Restore Verfahren

Dieses Kapitel beschreibt standardisierte Restore-Verfahren.

Restore ist ein kritischer Eingriff und darf nur kontrolliert durchgeführt werden.

---

### 9.1 DEV Restore

DEV Restore durchführen:

```bash
docker exec -i mariadb mariadb -u root -p emc_mitglieder_dev < emc_mitglieder_dev_backup.sql
```

Danach prüfen:

- Backend DEV startet
- Login funktioniert
- Mitgliederliste lädt
- fachliche Konsistenz prüfen

---

### 9.2 PROD Restore

PROD Restore durchführen:

```bash
docker exec -i mariadb mariadb -u root -p emc_mitglieder < emc_mitglieder_prod_backup.sql
```

Danach prüfen:

- Backend PROD startet
- Login funktioniert
- Daten konsistent
- fachliche Prüfung durchführen

---

### 9.3 Restore automatisierter Backups

Automatisierte Backups aus `mariadb-backup` liegen komprimiert als:

```text
.sql.gz
```

Beispiele:

```text
202605220300.emc_mitglieder.sql.gz
latest.emc_mitglieder.sql.gz
```

Vor Restore Datei entpacken:

```bash
gzip -dk latest.emc_mitglieder.sql.gz
```

oder bei Erhalt der Originaldatei:

```bash
gzip -dk 202605220300.emc_mitglieder.sql.gz
```

Danach Restore durchführen:

```bash
docker exec -i mariadb mariadb -u root -p emc_mitglieder < latest.emc_mitglieder.sql
```

Hinweis:

Automatisierte Backups enthalten den produktiven Datenstand zum Sicherungszeitpunkt.

---

### 9.4 Restore Nachkontrolle

Nach Restore prüfen:

Container Status:

```bash
docker ps
```

Backend Logs:

```bash
docker logs emc-mitglieder-backend-dev --tail 100
docker logs emc-mitglieder-backend-prod --tail 100
```

MariaDB Logs:

```bash
docker logs mariadb --tail 100
```

Monitoring:

- Uptime Kuma grün
- Telegram keine neuen Fehler

Fachlich prüfen:

- Login
- Mitgliederliste
- Detailansichten
- Datenkonsistenz
- Schreibfunktion / Auto-Save

---

### 9.5 Restore Sicherheitsregeln

Vor Restore immer prüfen:

- DEV oder PROD?
- richtige Datenbank?
- korrektes Backup?
- Backup vollständig?
- Auswirkungen verstanden?

Grundregel:

Restore niemals direkt unter Zeitdruck oder ohne Ursachenanalyse durchführen.

---

## 10. Deployment Recovery

Dieses Kapitel beschreibt Recovery bei fehlgeschlagenen Deployments.

---

### 10.1 Frontend Recovery

Typische Fälle:

- leere Seite
- Assets fehlen
- Deployment fehlerhaft
- falsches dist

Prüfen:

```bash
ls -la /volume1/docker/build/mitglieder-frontend-dev/dist
ls -la /volume1/docker/build/mitglieder-frontend-prod/dist
```

Recovery:

alten Build wiederherstellen

danach:

Portainer Redeploy

oder Container Neustart:

```bash
docker restart emc-mitglieder-frontend-dev
docker restart emc-mitglieder-frontend-prod
```

---

### 10.2 Backend Recovery

Typische Fälle:

- falsche JAR
- Build Fehler
- Spring Startfehler
- Deployment fehlgeschlagen

Prüfen:

```bash
find /volume1/docker/build/mitglieder-backend-dev -maxdepth 2 -type f -print
find /volume1/docker/build/mitglieder-backend-prod -maxdepth 2 -type f -print
```

Recovery:

korrekte JAR wiederherstellen

neu bauen:

DEV:

```bash
cd /volume1/docker/build/mitglieder-backend-dev
docker build -t emc-mitglieder-backend:dev .
```

PROD:

```bash
cd /volume1/docker/build/mitglieder-backend-prod
docker build -t emc-mitglieder-backend:prod .
```

danach:

Portainer Redeploy

oder Container Neustart:

```bash
docker restart emc-mitglieder-backend-dev
docker restart emc-mitglieder-backend-prod
```

---

### 10.3 Monitoring Recovery

Wenn Monitoring selbst fehlerhaft:

Uptime Kuma prüfen:

```bash
docker ps | grep uptime-kuma
docker logs uptime-kuma --tail 100
```

Neustart:

```bash
docker restart uptime-kuma
```

Telegram Test:

```bash
curl "https://api.telegram.org/bot<TOKEN>/getMe"
```

---

## 11. Datenbank Notfallmaßnahmen

Datenbankprobleme sind besonders kritisch, da sie produktive Mitgliedsdaten betreffen können.

Maßnahmen müssen kontrolliert und nachvollziehbar erfolgen.

---

### 11.1 Sofortmaßnahmen

Bei schweren Datenbankproblemen:

- keine unüberlegten Änderungen
- keine Tabellen löschen
- keine produktiven Daten überschreiben
- keine Blind-Restores
- Logs sichern
- Ursache eingrenzen

Grundregel:

Zuerst Diagnose, dann Eingriff.

---

### 11.2 MariaDB prüfen

Container Status:

```bash
docker ps | grep mariadb
```

Logs:

```bash
docker logs mariadb --tail 200
```

Interaktive Verbindung:

```bash
docker exec -it mariadb mariadb -u root -p
```

Datenbanken anzeigen:

```sql
SHOW DATABASES;
```

Verfügbare Tabellen prüfen:

```sql
USE emc_mitglieder;
SHOW TABLES;
```

---

### 11.3 Datenbankplatz prüfen

Speicherplatz prüfen:

```bash
df -h
```

Docker Speicher prüfen:

```bash
docker system df
```

Problemfälle:

- NAS Speicher voll
- Docker Speicher erschöpft
- Backup wächst unkontrolliert

---

### 11.4 Datenbankintegrität prüfen

Technische Schnellprüfung über phpMyAdmin:

Aktueller Zugriff:

```text
http://<NAS-IP>:8080
```

Prüfen:

- erwartete Datenbanken vorhanden
- Tabellen vorhanden
- Tabellenstruktur plausibel
- Datensätze vorhanden
- SQL Fehler sichtbar

Typische Nutzung:

- technische Analyse
- Read-Only Prüfungen
- Strukturkontrolle
- SQL Diagnose

> [!WARNING]
> Direkte produktive Datenänderungen nur im Ausnahmefall und mit technischer Sorgfalt.

---

### 11.5 Backup Infrastruktur prüfen

Backup Container:

```bash
docker ps | grep mariadb-backup
```

Backup Logs:

```bash
docker logs mariadb-backup --tail 100
```

Backup Dateien:

```bash
ls -lah /volume1/home/JaitiNissi1968/docker/backups/mariadb
```

Prüfen:

- aktuelle Zeitstempel vorhanden?
- `.sql.gz` Dateien vorhanden?
- `latest.*` Dateien vorhanden?
- Sicherungen plausibel?

---

### 11.6 Access Übergangssystem berücksichtigen

Aktuell existiert teilweise noch Access-basierter Altbetrieb.

Wichtig:

Produktive Datenänderungen können aktuell nicht ausschließlich über die Webanwendung erfolgen.

Bei Dateninkonsistenzen berücksichtigen:

- EMC Webanwendung
- Access Altanwendung
- manuelle technische Eingriffe
- Restore Maßnahmen

---

### 11.7 Eskalationsregel

Bei unklaren oder schwerwiegenden Problemen:

nicht weiter eskalieren durch spontane Eingriffe.

Stattdessen:

1. Ursache dokumentieren
2. Logs sichern
3. aktuellen Zustand festhalten
4. Backup Status prüfen
5. Recovery Strategie entscheiden

Grundregel:

Unkontrollierte Reparatur ist oft gefährlicher als der ursprüngliche Fehler.

---

## 12. Recovery Checkliste

Diese Checkliste dient als Standardverfahren im Störungsfall.

---

### 12.1 Erstdiagnose

Prüfen:

- Was ist betroffen?
- DEV oder PROD?
- Seit wann?
- letzte Änderung?
- Uptime Kuma Status?
- Telegram Alerts?

---

### 12.2 Technische Prüfung

Prüfen:

```bash
docker ps
docker ps -a
docker logs <betroffener-container>
docker inspect <betroffener-container>
docker network ls
df -h
free -h
```

---

### 12.3 Entscheidung

Fragen:

- nur Neustart nötig?
- Konfigurationsfehler?
- Deployment Problem?
- Datenbankproblem?
- Restore nötig?
- Rollback sinnvoll?

---

### 12.4 Nach Recovery

Pflicht:

- Smoke Test
- Monitoring prüfen
- Logs prüfen
- Ursache dokumentieren
- Folgefehler beobachten