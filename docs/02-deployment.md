# Deployment-Handbuch

## Inhaltsverzeichnis

- [Deployment-Handbuch](#deployment-handbuch)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [1. Zweck und Geltungsbereich](#1-zweck-und-geltungsbereich)
    - [1.1 Ziel des Dokuments](#11-ziel-des-dokuments)
    - [1.2 Zielgruppe](#12-zielgruppe)
    - [1.3 Abgrenzung zu anderen Dokumenten](#13-abgrenzung-zu-anderen-dokumenten)
  - [2. Deployment-Strategie](#2-deployment-strategie)
    - [2.1 Grundprinzip](#21-grundprinzip)
    - [2.2 Deployment-Prinzipien](#22-deployment-prinzipien)
    - [2.3 Verantwortungsmodell](#23-verantwortungsmodell)
    - [2.4 Warum kein CI/CD](#24-warum-kein-cicd)
  - [3. Deployment-Standards und Build-Pfade](#3-deployment-standards-und-build-pfade)
    - [3.1 Verbindliche NAS Build-Struktur](#31-verbindliche-nas-build-struktur)
    - [3.2 Frontend Struktur](#32-frontend-struktur)
    - [3.3 Backend Struktur](#33-backend-struktur)
    - [3.4 Windows Entwicklungsmodell](#34-windows-entwicklungsmodell)
    - [3.5 Wichtiger Pfad-Hinweis](#35-wichtiger-pfad-hinweis)
    - [3.6 Rechtevoraussetzungen](#36-rechtevoraussetzungen)
    - [3.7 Keine Container-Manipulation](#37-keine-container-manipulation)
  - [4. Frontend Deployment](#4-frontend-deployment)
    - [4.1 Übersicht](#41-übersicht)
    - [4.2 Lokaler Build](#42-lokaler-build)
    - [4.3 Zielpfade](#43-zielpfade)
    - [4.4 Transfer auf NAS](#44-transfer-auf-nas)
      - [Vorheriges Frontend-Artefakt prüfen](#vorheriges-frontend-artefakt-prüfen)
      - [Vorheriges Frontend-Artefakt archivieren](#vorheriges-frontend-artefakt-archivieren)
      - [Zielverzeichnis bereinigen](#zielverzeichnis-bereinigen)
      - [Neues Frontend-Artefakt übertragen](#neues-frontend-artefakt-übertragen)
      - [Rechte korrigieren](#rechte-korrigieren)
      - [Übertragung prüfen](#übertragung-prüfen)
      - [Container aktualisieren](#container-aktualisieren)
    - [4.5 Portainer Deployment](#45-portainer-deployment)
    - [4.6 Frontend Smoke Test](#46-frontend-smoke-test)
    - [4.7 Typische Frontend Fehler](#47-typische-frontend-fehler)
      - [Leere Seite](#leere-seite)
      - [Alte Version sichtbar](#alte-version-sichtbar)
      - [API Fehler](#api-fehler)
  - [5. Backend Deployment](#5-backend-deployment)
    - [5.1 Übersicht](#51-übersicht)
    - [5.2 Lokaler Build](#52-lokaler-build)
    - [5.3 Zielpfade](#53-zielpfade)
    - [5.4 Dockerfile Standard](#54-dockerfile-standard)
    - [5.5 Transfer auf NAS](#55-transfer-auf-nas)
      - [Vorheriges Artefakt prüfen](#vorheriges-artefakt-prüfen)
      - [Vorheriges Artefakt archivieren](#vorheriges-artefakt-archivieren)
      - [Build-Verzeichnis bereinigen](#build-verzeichnis-bereinigen)
      - [Neues Artefakt übertragen](#neues-artefakt-übertragen)
      - [Docker Build Bezug](#docker-build-bezug)
    - [5.6 Docker Build](#56-docker-build)
    - [5.7 Portainer Deployment](#57-portainer-deployment)
    - [5.8 Backend Smoke Test](#58-backend-smoke-test)
    - [5.9 Typische Backend Fehler](#59-typische-backend-fehler)
      - [Container startet nicht](#container-startet-nicht)
      - [Datenbankverbindung schlägt fehl](#datenbankverbindung-schlägt-fehl)
      - [Falscher Code läuft](#falscher-code-läuft)
  - [6. Datenbankänderungen](#6-datenbankänderungen)
    - [6.1 Grundprinzip](#61-grundprinzip)
    - [6.2 Typische Datenbankänderungen](#62-typische-datenbankänderungen)
    - [6.3 Backup vor Änderungen](#63-backup-vor-änderungen)
    - [6.4 DEV Validierung](#64-dev-validierung)
    - [6.5 Aktueller Migrationsstatus](#65-aktueller-migrationsstatus)
  - [7. DEV → PROD Release Prozess](#7-dev--prod-release-prozess)
    - [7.1 Standardprozess](#71-standardprozess)
    - [7.2 Frontend Release Ablauf](#72-frontend-release-ablauf)
    - [7.3 Backend Release Ablauf](#73-backend-release-ablauf)
    - [7.4 Release mit Datenbankänderungen](#74-release-mit-datenbankänderungen)
    - [7.5 Release Freigaberegel](#75-release-freigaberegel)
  - [8. Smoke Tests](#8-smoke-tests)
    - [8.1 Frontend Smoke Test](#81-frontend-smoke-test)
    - [8.2 Backend Smoke Test](#82-backend-smoke-test)
    - [8.3 Fachlicher Smoke Test](#83-fachlicher-smoke-test)
    - [8.4 Schreibtest](#84-schreibtest)
    - [8.5 Monitoring](#85-monitoring)
    - [8.6 Versionsprüfung](#86-versionsprüfung)
    - [8.7 Logprüfung](#87-logprüfung)
  - [9. Rollback-Grundprinzip](#9-rollback-grundprinzip)
    - [9.1 Grundprinzip](#91-grundprinzip)
    - [9.2 Frontend Rollback](#92-frontend-rollback)
    - [9.3 Backend Rollback](#93-backend-rollback)
    - [9.4 Datenbank Rollback](#94-datenbank-rollback)
    - [9.5 Zukünftige Archivstrategie](#95-zukünftige-archivstrategie)
  - [10. Typische Fehlerquellen](#10-typische-fehlerquellen)
    - [10.1 Falscher NAS Pfad](#101-falscher-nas-pfad)
    - [10.2 Rechteprobleme](#102-rechteprobleme)
    - [10.3 Falsche Zielumgebung](#103-falsche-zielumgebung)
    - [10.4 Alte Artefakte](#104-alte-artefakte)
    - [10.5 Falscher Stack Redeploy](#105-falscher-stack-redeploy)
    - [10.6 Datenbank vergessen](#106-datenbank-vergessen)
    - [10.7 Smoke Test ausgelassen](#107-smoke-test-ausgelassen)
    - [10.8 Direkte Container-Manipulation](#108-direkte-container-manipulation)

---

## 1. Zweck und Geltungsbereich

### 1.1 Ziel des Dokuments

Dieses Dokument beschreibt den standardisierten Deployment-Prozess für die EMC Mitgliederverwaltung.

Ziel ist ein reproduzierbarer, nachvollziehbarer und sicherer technischer Ausrollprozess für Änderungen an Frontend, Backend und zugehörigen Infrastrukturkomponenten.

Das Dokument beschreibt den aktuell gültigen manuellen Deployment-Standard für den Betrieb auf dem UGREEN DH2300 NAS mit Docker und Portainer.

---

### 1.2 Zielgruppe

Dieses Dokument richtet sich an technisch verantwortliche Personen.

Insbesondere:

- technischer Betreiber
- Entwickler
- Administratoren
- zukünftige technische Nachfolger im Projekt

Das Dokument ist bewusst schrittweise und praktisch aufgebaut, damit Deployments auch nach längerer Zeit sicher nachvollzogen werden können.

---

### 1.3 Abgrenzung zu anderen Dokumenten

Dieses Dokument beschreibt den Release- und Deployment-Prozess.

Nicht Bestandteil:

| Thema                                | Dokument                               |
| ------------------------------------ | -------------------------------------- |
| Installation und Betrieb             | `01-installation-betrieb.md`           |
| Benutzerbedienung                    | `03-benutzerhandbuch.md`               |
| Architektur                          | `04-architektur.md`                    |
| Troubleshooting / Recovery / Restore | `05-troubleshooting-backup-restore.md` |

---

## 2. Deployment-Strategie

Die EMC Mitgliederverwaltung folgt einem bewusst pragmatischen, manuell kontrollierten Deployment-Modell.

Es existiert aktuell keine automatisierte CI/CD-Pipeline.

Deployments erfolgen kontrolliert durch den technischen Betreiber.

---

### 2.1 Grundprinzip

Standardprozess:

```text
Lokale Entwicklung
→ lokaler Build
→ kontrollierter Transfer auf NAS
→ Docker Build oder Portainer Redeploy
→ Smoke Test
```

Das Deployment erfolgt immer zuerst in der DEV-Umgebung.

PROD wird erst nach erfolgreicher DEV-Prüfung aktualisiert.

---

### 2.2 Deployment-Prinzipien

Verbindliche Grundregeln:

- DEV zuerst
- PROD erst nach erfolgreichem DEV-Test
- keine Änderungen direkt in laufenden Containern
- keine manuellen Container-Hacks
- keine nicht dokumentierten Sonderwege
- reproduzierbare Standardprozesse verwenden
- Smoke Tests sind Pflicht
- Build-Pfade strikt einhalten

---

### 2.3 Verantwortungsmodell

Deployments erfolgen aktuell manuell.

Verantwortlich:

- technischer Betreiber

Das Deployment-Modell ist bewusst nachvollziehbar dokumentiert, damit es auch nach längerer Zeit reproduzierbar durchgeführt werden kann.

---

### 2.4 Warum kein CI/CD

Das Projekt befindet sich aktuell in einem pragmatischen MVP-/Pilotbetriebsmodell.

Eine manuelle Deployment-Strategie ist derzeit bewusst gewählt, weil sie:

- transparent
- nachvollziehbar
- kontrollierbar
- einfach wartbar
- ohne externe Infrastruktur durchführbar

ist.

Eine spätere Automatisierung ist möglich.

---

## 3. Deployment-Standards und Build-Pfade

Dieses Kapitel definiert die verbindlichen technischen Deployment-Standards.

---

### 3.1 Verbindliche NAS Build-Struktur

Aktueller Standard:

```text
/volume1/docker/build/
├── mitglieder-backend-dev
├── mitglieder-backend-prod
├── mitglieder-frontend-dev
└── mitglieder-frontend-prod
```

Diese Struktur ist verbindlich.

Sie ersetzt ältere historisch gewachsene Build-Pfade unterhalb des Benutzer-Home-Verzeichnisses.

---

### 3.2 Frontend Struktur

Frontend DEV:

```text
/volume1/docker/build/mitglieder-frontend-dev
```

Frontend PROD:

```text
/volume1/docker/build/mitglieder-frontend-prod
```

Struktur je Umgebung:

```text
dist/
nginx.conf
```

Bedeutung:

| Element      | Zweck                               |
| ------------ | ----------------------------------- |
| `dist/`      | gebautes React/Vite Frontend        |
| `nginx.conf` | nginx Konfiguration inkl. API Proxy |

---

### 3.3 Backend Struktur

Backend DEV:

```text
/volume1/docker/build/mitglieder-backend-dev
```

Backend PROD:

```text
/volume1/docker/build/mitglieder-backend-prod
```

Struktur je Umgebung:

```text
Dockerfile
target/*.jar
```

Bedeutung:

| Element        | Zweck                                 |
| -------------- | ------------------------------------- |
| `Dockerfile`   | Build Definition für Backend Image    |
| `target/*.jar` | gebautes Spring Boot Backend Artefakt |

---

### 3.4 Windows Entwicklungsmodell

Die Entwicklung erfolgt lokal unter Windows.

Frontend:

- VS Code
- React / Vite
- lokaler Build mit `npm run build`

Backend:

- IntelliJ IDEA
- Maven
- Spring Boot
- lokaler Build mit `mvn clean package`

Build-Artefakte entstehen lokal und werden anschließend kontrolliert auf das NAS übertragen.

---

### 3.5 Wichtiger Pfad-Hinweis

NAS Weboberfläche, Windows Explorer und Linux SSH können unterschiedliche Pfadsichten erzeugen.

Verbindlich für Deployment sind ausschließlich die tatsächlich von Docker verwendeten Pfade.

Prüfung:

```bash
docker inspect <containername>
```

Typischer Fehler:

```text
GUI sichtbarer Pfad ≠ Docker Bind Mount Pfad
```

> [!WARNING]
> Für Deployment und Troubleshooting immer die Linux-/Docker-Pfade verwenden, nicht ausschließlich die NAS-GUI-Ansicht.

---

### 3.6 Rechtevoraussetzungen

Der operative Deployment-Benutzer benötigt Schreibrechte auf die NAS Build-Verzeichnisse.

Insbesondere:

```text
/volume1/docker/build
```

Typische Symptome bei fehlenden Rechten:

```text
Permission denied
```

Beispiel:

```bash
mkdir: cannot create directory ... Permission denied
```

Prüfung:

```bash
ls -ld /volume1/docker/build
```

Beispiel korrekter Benutzerrechte:

```text
drwxr-xr-x JaitiNissi1968 users /volume1/docker/build
```

Typische Einrichtung:

```bash
sudo chown -R JaitiNissi1968:users /volume1/docker/build
```

Ziel:

- Deployments ohne root-Sonderwege
- konsistente Schreibrechte
- reproduzierbarer operativer Ablauf

---

### 3.7 Keine Container-Manipulation

Nicht zulässig:

- direkte Änderungen in laufenden Containern
- manuelles Kopieren in Container
- `docker exec` Dateioperationen als Deployment-Ersatz
- `docker cp` als regulärer Deployment-Weg

Beispiele unerwünschter Methoden:

```bash
docker cp
docker exec
```

Deployments erfolgen ausschließlich über:

- definierte Build-Pfade
- Docker Image Builds
- Portainer Stack Redeployments

> [!NOTE]
> Änderungen innerhalb laufender Container sind nicht dauerhaft und gehen beim Redeployment verloren.

---

## 4. Frontend Deployment

Dieses Kapitel beschreibt den standardisierten Deployment-Prozess für das React-Frontend.

---

### 4.1 Übersicht

Frontend Deployment erfolgt über statische Build-Artefakte.

Prinzip:

```text
lokaler Windows Build
→ dist auf NAS kopieren
→ Portainer Frontend Stack redeployen
→ Smoke Test
```

---

### 4.2 Lokaler Build

Im Frontend-Projekt lokal unter Windows:

```bash
npm run build
```

Erwartetes Ergebnis:

```text
dist/
```

Beispiel:

```text
dist/
├── assets/
├── index.html
├── favicon.svg
└── icons.svg
```

---

### 4.3 Zielpfade

DEV:

```text
/volume1/docker/build/mitglieder-frontend-dev/dist
```

PROD:

```text
/volume1/docker/build/mitglieder-frontend-prod/dist
```

Zusätzliche Konfiguration:

DEV:

```text
/volume1/docker/build/mitglieder-frontend-dev/nginx.conf
```

PROD:

```text
/volume1/docker/build/mitglieder-frontend-prod/nginx.conf
```

---

### 4.4 Transfer auf NAS

Lokale Frontend Build-Artefakte werden kontrolliert auf das NAS übertragen.

Nach lokalem Build:

```bash
npm run build
```

entsteht lokal:

```text
dist/
```

Dieses Verzeichnis wird vollständig auf das NAS übertragen.

DEV Ziel:

```text
/volume1/docker/build/mitglieder-frontend-dev/dist
```

PROD Ziel:

```text
/volume1/docker/build/mitglieder-frontend-prod/dist
```

---

#### Vorheriges Frontend-Artefakt prüfen

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-frontend-dev
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-frontend-prod
```

---

#### Vorheriges Frontend-Artefakt archivieren

Archivverzeichnis sicherstellen:

```bash
mkdir -p /volume1/docker/build/archive/frontend
```

Zeitstempel erzeugen:

```bash
TIMESTAMP=$(date +%Y-%m-%d_%H-%M)
```

Wichtig:

Das bestehende `dist` Verzeichnis darf bei laufendem Container **nicht per `mv` verschoben** werden.

Grund:

Der nginx Container verwendet einen Bind Mount auf dieses Verzeichnis.

Ein Verschieben würde dazu führen, dass der laufende Container weiterhin auf das alte Verzeichnis zeigt.

Deshalb vollständige Kopie erstellen.

DEV:

```bash
if [ -d /volume1/docker/build/mitglieder-frontend-dev/dist ]; then
  cp -a /volume1/docker/build/mitglieder-frontend-dev/dist \
        /volume1/docker/build/archive/frontend/dist-dev-${TIMESTAMP}
fi
```

PROD:

```bash
if [ -d /volume1/docker/build/mitglieder-frontend-prod/dist ]; then
  cp -a /volume1/docker/build/mitglieder-frontend-prod/dist \
        /volume1/docker/build/archive/frontend/dist-prod-${TIMESTAMP}
fi
```

Beispiel:

```text
dist
```

wird archiviert als:

```text
dist-dev-2026-05-23_20-15
```

Ziel:

- vollständige Frontend-Historie
- Rollback-Möglichkeit
- kein Bind-Mount-Problem

---

#### Zielverzeichnis bereinigen

Nach erfolgreicher Archivierung Inhalte des bestehenden `dist` Verzeichnisses entfernen.

DEV:

```bash
rm -rf /volume1/docker/build/mitglieder-frontend-dev/dist/*
```

PROD:

```bash
rm -rf /volume1/docker/build/mitglieder-frontend-prod/dist/*
```

Kontrolle:

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-frontend-dev/dist
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-frontend-prod/dist
```

Das `dist` Verzeichnis selbst bleibt bestehen.

Nur dessen Inhalt wird ersetzt.

---

#### Neues Frontend-Artefakt übertragen

Beispiel via SCP (Windows PowerShell):

DEV:

```powershell
scp -r dist/* JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-frontend-dev/dist/
```

PROD:

```powershell
scp -r dist/* JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-frontend-prod/dist/
```

---

#### Rechte korrigieren

Nach SCP Transfer Verzeichnisrechte korrigieren.

Grund:

Lokale Windows/SCP Rechte können dazu führen, dass nginx keinen Zugriff auf das Frontend erhält.

Typischer Fehler:

```text
500 Internal Server Error
Permission denied
stat() "/usr/share/nginx/html/index.html"
```

Korrektur:

DEV:

```bash
chmod -R 755 /volume1/docker/build/mitglieder-frontend-dev/dist
```

PROD:

```bash
chmod -R 755 /volume1/docker/build/mitglieder-frontend-prod/dist
```

---

#### Übertragung prüfen

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-frontend-dev/dist
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-frontend-prod/dist
```

Erwartet:

```text
assets/
index.html
favicon.svg
icons.svg
```

---

#### Container aktualisieren

Frontend Container neu deployen.

Portainer:

```text
Frontend DEV / PROD Stack
→ Redeploy
```

---

> [!WARNING]
> Frontend-Artefakte immer vollständig ersetzen.

Nicht zulässig:

- selektive Einzeldateien austauschen
- dist per `mv` bei laufendem Container verschieben
- Rechteprobleme ignorieren

---

### 4.5 Portainer Deployment

Frontend verwendet keine eigenen Images.

Das Frontend läuft als nginx Container mit Bind Mounts.

DEV Mounts:

```text
/volume1/docker/build/mitglieder-frontend-dev/dist
→ /usr/share/nginx/html

/volume1/docker/build/mitglieder-frontend-dev/nginx.conf
→ /etc/nginx/conf.d/default.conf
```

PROD Mounts:

```text
/volume1/docker/build/mitglieder-frontend-prod/dist
→ /usr/share/nginx/html

/volume1/docker/build/mitglieder-frontend-prod/nginx.conf
→ /etc/nginx/conf.d/default.conf
```

Deployment:

Portainer öffnen

entsprechenden Stack auswählen:

- Frontend DEV
  oder
- Frontend PROD

Aktion:

```text
Redeploy
```

---

### 4.6 Frontend Smoke Test

Nach Deployment prüfen:

- Loginseite lädt
- CSS / JS Assets laden korrekt
- Navigation funktioniert
- API Kommunikation funktioniert
- Login möglich
- Mitgliederliste lädt
- Detailseite öffnet

Optional technisch:

Container Logs prüfen:

```bash
docker logs emc-mitglieder-frontend-dev --tail 50
```

oder:

```bash
docker logs emc-mitglieder-frontend-prod --tail 50
```

---

### 4.7 Typische Frontend Fehler

#### Leere Seite

Mögliche Ursache:

- dist unvollständig
- falscher Zielpfad
- nginx Mount falsch

---

#### Alte Version sichtbar

Mögliche Ursache:

- Browser Cache
- dist nicht vollständig ersetzt
- falscher Deployment-Pfad

---

#### API Fehler

Mögliche Ursache:

- nginx.conf falsch
- Backend Container nicht erreichbar
- Proxy Ziel falsch

DEV:

```text
emc-mitglieder-backend-dev
```

PROD:

```text
emc-mitglieder-backend-prod
```

---

## 5. Backend Deployment

Dieses Kapitel beschreibt den standardisierten Deployment-Prozess für das Spring Boot Backend.

---

### 5.1 Übersicht

Backend Deployment erfolgt image-basiert.

Prinzip:

```text
lokaler Windows Maven Build
→ JAR auf NAS kopieren
→ Docker Image bauen
→ Portainer Backend Stack redeployen
→ Smoke Test
```

---

### 5.2 Lokaler Build

Im Backend-Projekt lokal unter Windows:

```bash
mvn clean package
```

Erwartetes Ergebnis:

```text
target/*.jar
```

Beispiel:

```text
target/mitglieder-backend-1.1.1-SNAPSHOT.jar
```

---

### 5.3 Zielpfade

DEV:

```text
/volume1/docker/build/mitglieder-backend-dev
```

PROD:

```text
/volume1/docker/build/mitglieder-backend-prod
```

Struktur:

```text
Dockerfile
target/*.jar
```

---

### 5.4 Dockerfile Standard

Verbindlicher Standard:

```dockerfile
FROM eclipse-temurin:21-jre

WORKDIR /app

COPY target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

---

### 5.5 Transfer auf NAS

Nach lokalem Maven Build:

```bash
mvn clean package
```

wird das erzeugte Backend-Artefakt auf das NAS übertragen.

DEV Ziel:

```text
/volume1/docker/build/mitglieder-backend-dev/target
```

PROD Ziel:

```text
/volume1/docker/build/mitglieder-backend-prod/target
```

---

#### Vorheriges Artefakt prüfen

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-dev/target
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-prod/target
```

---

#### Vorheriges Artefakt archivieren

Archivverzeichnis sicherstellen:

```bash
mkdir -p /volume1/docker/build/archive/backend
```

Zeitstempel erzeugen:

```bash
TIMESTAMP=$(date +%Y-%m-%d_%H-%M)
```

DEV:

```bash
for f in /volume1/docker/build/mitglieder-backend-dev/target/*.jar; do
  [ -e "$f" ] || continue
  BASENAME=$(basename "$f" .jar)
  mv "$f" "/volume1/docker/build/archive/backend/${BASENAME}-${TIMESTAMP}.jar"
done
```

PROD:

```bash
for f in /volume1/docker/build/mitglieder-backend-prod/target/*.jar; do
  [ -e "$f" ] || continue
  BASENAME=$(basename "$f" .jar)
  mv "$f" "/volume1/docker/build/archive/backend/${BASENAME}-${TIMESTAMP}.jar"
done
```

Beispiel:

```text
mitglieder-backend-1.1.1-SNAPSHOT.jar
```

wird zu:

```text
mitglieder-backend-1.1.1-SNAPSHOT-2026-05-22_21-45.jar
```

Ziel:

- nachvollziehbare Deployment-Historie
- Rollback-Möglichkeit
- keine Verwechslungsgefahr

---

#### Build-Verzeichnis bereinigen

Nach erfolgreicher Archivierung sicherstellen, dass kein Altartefakt mehr vorhanden ist.

DEV:

```bash
rm -f /volume1/docker/build/mitglieder-backend-dev/target/*.jar
```

PROD:

```bash
rm -f /volume1/docker/build/mitglieder-backend-prod/target/*.jar
```

Kontrolle:

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-dev/target
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-prod/target
```

Im target-Verzeichnis darf nur das aktuell zu deployende Backend-Artefakt liegen.

---

#### Neues Artefakt übertragen

Beispiel via SCP (Windows PowerShell):

DEV:

```powershell
scp target/mitglieder-backend-*.jar JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-backend-dev/target/
```

PROD:

```powershell
scp target/mitglieder-backend-*.jar JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-backend-prod/target/
```

Kontrolle:

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-dev/target
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-prod/target
```

---

#### Docker Build Bezug

Der Docker Build verwendet:

```dockerfile
COPY target/*.jar app.jar
```

Dadurch sind Versionsänderungen wie:

```text
mitglieder-backend-1.1.1-SNAPSHOT.jar
mitglieder-backend-1.1.2-SNAPSHOT.jar
```

problemlos, solange sich genau **eine gültige JAR-Datei** im target-Verzeichnis befindet.

---

### 5.6 Docker Build

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

---

### 5.7 Portainer Deployment

Nach erfolgreichem Image Build:

Portainer öffnen

entsprechenden Stack auswählen:

- Backend DEV
  oder
- Backend PROD

Aktion:

```text
Redeploy
```

---

### 5.8 Backend Smoke Test

Prüfen:

- Login funktioniert
- Session Restore funktioniert
- Mitgliederliste lädt
- Suche funktioniert
- Detailansicht lädt
- Auto-Save funktioniert

Technisch:

Logs prüfen:

DEV:

```bash
docker logs emc-mitglieder-backend-dev --tail 80
```

PROD:

```bash
docker logs emc-mitglieder-backend-prod --tail 80
```

Erwartet:

- Spring Boot Start erfolgreich
- DB Verbindung erfolgreich
- keine Exceptions

---

### 5.9 Typische Backend Fehler

#### Container startet nicht

Mögliche Ursache:

- JAR fehlt
- Dockerfile fehlerhaft
- falscher Image Tag

---

#### Datenbankverbindung schlägt fehl

Mögliche Ursache:

- MariaDB nicht erreichbar
- falsche ENV Werte
- Netzwerkproblem

---

#### Falscher Code läuft

Mögliche Ursache:

- falsche JAR kopiert
- Build nicht aktualisiert
- falscher Tag verwendet

---

## 6. Datenbankänderungen

Datenbankänderungen erfordern besondere Sorgfalt.

Die EMC Mitgliederverwaltung arbeitet produktiv mit realen Daten.

Zusätzlich existiert aktuell noch eine Access-Übergangsarchitektur mit direktem Datenbankzugriff.

---

### 6.1 Grundprinzip

Regeln:

- Datenbankänderungen zuerst in DEV
- technische Prüfung in DEV
- funktionale Prüfung in DEV
- Backup vor PROD Änderungen
- PROD nur kontrolliert

---

### 6.2 Typische Datenbankänderungen

Beispiele:

- neue Tabellen
- neue Spalten
- Indexänderungen
- Constraint Änderungen
- Lookup Erweiterungen
- Benutzer / Rechte Änderungen

---

### 6.3 Backup vor Änderungen

Vor Änderungen in PROD zwingend Backup erstellen.

Mindestens:

```text
MariaDB Datenbank Backup
```

Aktuelle Backup-Infrastruktur:

automatisierter Docker Container:

```text
mariadb-backup
```

Tägliche Sicherung der relevanten MariaDB Datenbanken.

Bei kritischen Änderungen zusätzlich empfohlen:

manuelles Zusatzbackup.

---

### 6.4 DEV Validierung

Vor PROD prüfen:

- Anwendung startet
- Login funktioniert
- relevante Funktionen funktionieren
- CRUD Prozesse funktionieren
- Auto-Save funktioniert
- Fehlerfälle prüfen

---

### 6.5 Aktueller Migrationsstatus

Aktuell existiert keine automatisierte Schema-Migration (z. B. Flyway/Liquibase).

Datenbankänderungen erfolgen kontrolliert und manuell.

> [!WARNING]
> Datenbankänderungen müssen besonders sorgfältig dokumentiert und geprüft werden.

---

## 7. DEV → PROD Release Prozess

Dieses Kapitel beschreibt den standardisierten Release-Ablauf.

---

### 7.1 Standardprozess

Verbindlicher Ablauf:

```text
Entwicklung
→ lokaler Build
→ DEV Deployment
→ DEV Smoke Test
→ fachliche Prüfung
→ PROD Vorbereitung
→ PROD Deployment
→ PROD Smoke Test
→ Monitoring Kontrolle
```

---

### 7.2 Frontend Release Ablauf

1. lokale Änderungen abschließen

2. Frontend Build:

```bash
npm run build
```

3. DEV Deployment

4. DEV Smoke Test

5. PROD Deployment

6. PROD Smoke Test

---

### 7.3 Backend Release Ablauf

1. lokale Änderungen abschließen

2. Backend Build:

```bash
mvn clean package
```

3. DEV Deployment

4. DEV Smoke Test

5. PROD Deployment

6. PROD Smoke Test

---

### 7.4 Release mit Datenbankänderungen

Zusätzlicher Ablauf:

1. DEV DB Änderung
2. DEV Anwendungstest
3. PROD Backup
4. PROD DB Änderung
5. Backend Deployment
6. Smoke Test

---

### 7.5 Release Freigaberegel

PROD Deployment nur wenn:

- Monitoring zeigt stabile Zielumgebung
- DEV erfolgreich
- Smoke Tests erfolgreich
- Build-Artefakte geprüft
- richtige Zielumgebung gewählt
- Datenbankänderungen abgesichert

---

## 8. Smoke Tests

Smoke Tests sind Pflicht nach jedem Deployment.

---

### 8.1 Frontend Smoke Test

Prüfen:

- Loginseite lädt
- Assets laden
- CSS korrekt
- Navigation funktioniert
- Browser Console ohne relevante Fehler

---

### 8.2 Backend Smoke Test

Prüfen:

- Login funktioniert
- Session Restore funktioniert (`/api/auth/me`)
- API Kommunikation funktioniert
- keine Serverfehler sichtbar

---

### 8.3 Fachlicher Smoke Test

Prüfen:

- Mitgliederliste lädt
- Suche funktioniert
- Filter funktionieren
- Detailansicht öffnet
- Stammdaten laden
- Kontaktdaten laden
- Mitgliedschaft laden
- Datenschutz laden
- Chorkleidung laden

---

### 8.4 Schreibtest

Nach Änderungen möglichst prüfen:

- Feldänderung durchführen
- Auto-Save erfolgreich
- Reload prüfen
- Persistenz prüfen

---

### 8.5 Monitoring

Nach Deployment prüfen:

Uptime Kuma:

- Frontend DEV / PROD
- Backend DEV / PROD
- Datenbank
- Infrastruktur

---

### 8.6 Versionsprüfung

Nach Deployment Versionsanzeige prüfen.

Frontend:

- Frontend-Version sichtbar
- erwartete Version korrekt

Backend:

- Backend-Version sichtbar
- erwartete Version korrekt

Beispiel:

```text
FE v1.0.0-SNAPSHOT | BE v1.1.1-SNAPSHOT
```

Zweck:

- schneller technischer Deployment-Sanity-Check
- Erkennung falscher Builds
- Erkennung fehlender Redeployments
- Erkennung von Frontend-/Backend-Versionsabweichungen

Wenn Versionen nicht erwartungsgemäß erscheinen:

- falscher Build deployed
- Container nicht redeployed
- Browser Cache aktiv
- falsche Zielumgebung verwendet

---

### 8.7 Logprüfung

Frontend:

```bash
docker logs emc-mitglieder-frontend-dev --tail 50
docker logs emc-mitglieder-frontend-prod --tail 50
```

Backend:

```bash
docker logs emc-mitglieder-backend-dev --tail 80
docker logs emc-mitglieder-backend-prod --tail 80
```

Prüfen:

- Start erfolgreich
- keine Exceptions
- keine unerwarteten Fehler

---

## 9. Rollback-Grundprinzip

---

### 9.1 Grundprinzip

Rollback erfolgt aktuell manuell.

Möglichkeiten:

- archivierte Build-Artefakte wiederherstellen
- Backend Image neu erzeugen
- Stack redeployen
- Datenbank Restore (wenn notwendig)

---

### 9.2 Frontend Rollback

Prinzip:

älteres dist wiederherstellen

danach:

```text
Portainer Redeploy
```

---

### 9.3 Backend Rollback

Prinzip:

ältere JAR wiederherstellen

neu bauen:

```bash
docker build -t emc-mitglieder-backend:dev .
```

oder PROD:

```bash
docker build -t emc-mitglieder-backend:prod .
```

danach:

```text
Portainer Redeploy
```

---

### 9.4 Datenbank Rollback

Besondere Vorsicht:

Datenbank Rollback ist komplexer.

Mögliche Maßnahmen:

- Restore Backup
- gezielte SQL Korrektur
- manuelle Reparatur

---

### 9.5 Zukünftige Archivstrategie

Geplante, aktuell noch nicht umgesetzte Option:

dateibasierte Archivierung früherer Build-Stände.

Beispiel:

```text
/volume1/docker/archive/
```

oder:

```text
/volume1/docker/build/archive/
```

Dies vereinfacht spätere Rollbacks.

---

## 10. Typische Fehlerquellen

Dieses Kapitel dokumentiert bekannte praktische Fehlerquellen.

---

### 10.1 Falscher NAS Pfad

Problem:

GUI sichtbarer Pfad ≠ Docker Pfad

Lösung:

```bash
docker inspect <container>
```

---

### 10.2 Rechteprobleme

Typisch:

```text
Permission denied
```

Ursache:

Owner / Group Rechte auf NAS.

Typischer Fix:

```bash
sudo chown -R <user>:users <pfad>
```

---

### 10.3 Falsche Zielumgebung

Typisch:

DEV statt PROD oder umgekehrt.

Vermeidung:

Pfadnamen strikt beachten:

```text
*-dev
*-prod
```

---

### 10.4 Alte Artefakte

Typisch:

- alte dist
- alte JAR
- falscher Build

Vermeidung:

Artefakte bewusst ersetzen.

---

### 10.5 Falscher Stack Redeploy

Typisch:

falschen Portainer Stack aktualisiert.

Prüfen:

- DEV oder PROD?
- Frontend oder Backend?

---

### 10.6 Datenbank vergessen

Typisch:

Code deployed, DB Änderung vergessen.

Folge:

Anwendung startet nicht oder Fehler.

---

### 10.7 Smoke Test ausgelassen

Typisch:

Deployment erfolgreich angenommen, aber nicht geprüft.

Regel:

Smoke Tests sind Pflicht.

---

### 10.8 Direkte Container-Manipulation

Typisch:

manuelle Änderungen in laufenden Containern.

Folge:

Änderungen verschwinden beim nächsten Redeploy.

Nicht verwenden:

```bash
docker exec
docker cp
```
