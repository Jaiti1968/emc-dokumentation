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
    - [4.2 Versionsvorbereitung](#42-versionsvorbereitung)
    - [4.3 Lokaler Build](#43-lokaler-build)
    - [4.4 Docker Image Build](#44-docker-image-build)
      - [DEV Build](#dev-build)
      - [PROD Build](#prod-build)
    - [4.5 Image Archivierung](#45-image-archivierung)
      - [DEV](#dev)
      - [PROD](#prod)
    - [4.6 Transfer auf NAS](#46-transfer-auf-nas)
      - [DEV](#dev-1)
      - [PROD](#prod-1)
    - [4.7 Image Import auf NAS](#47-image-import-auf-nas)
      - [DEV](#dev-2)
      - [PROD](#prod-2)
    - [4.8 Portainer Deployment](#48-portainer-deployment)
      - [Deployment-Konfiguration prüfen](#deployment-konfiguration-prüfen)
    - [4.9 Frontend Smoke Test](#49-frontend-smoke-test)
    - [4.10 Typische Frontend Fehler](#410-typische-frontend-fehler)
      - [Falsche Versionsanzeige](#falsche-versionsanzeige)
      - [Container startet nicht](#container-startet-nicht)
      - [exec format error](#exec-format-error)
      - [Falsche Backend-Umgebung](#falsche-backend-umgebung)
      - [Alte Version sichtbar](#alte-version-sichtbar)
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
      - [Deployment-Konfiguration prüfen](#deployment-konfiguration-prüfen-1)
    - [5.7 Portainer Deployment](#57-portainer-deployment)
    - [5.8 Backend Smoke Test](#58-backend-smoke-test)
    - [5.9 Typische Backend Fehler](#59-typische-backend-fehler)
      - [Container startet nicht](#container-startet-nicht-1)
      - [Datenbankverbindung schlägt fehl](#datenbankverbindung-schlägt-fehl)
      - [Falscher Code läuft](#falscher-code-läuft)
      - [Falsche Version läuft](#falsche-version-läuft)
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
    - [9.5 Aktuelle Archivstrategie](#95-aktuelle-archivstrategie)
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
image/
```

Beispiel:

```text
image/
└── emc-mitglieder-frontend-1.1.1.tar
```

Bedeutung:

| Element  | Zweck                                           |
| -------- | ----------------------------------------------- |
| `image/` | Docker Image Archiv für Deployment und Rollback |

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

Dieses Kapitel beschreibt den standardisierten image-basierten Deployment-Prozess für das React-Frontend.

---

### 4.1 Übersicht

Frontend Deployment erfolgt image-basiert.

Prinzip:

```text
Versionsprüfung
→ lokaler Build
→ Docker Image Build
→ Docker Image Export
→ Transfer auf NAS
→ Docker Image Import
→ Portainer Redeploy
→ Smoke Test
```

DEV verwendet Snapshot-Versionen:

```text
X.Y.Z-SNAPSHOT
```

PROD verwendet Release-Versionen:

```text
X.Y.Z
```

Frontend Images werden für die NAS-Zielplattform gebaut:

```text
linux/arm64
```

---

### 4.2 Versionsvorbereitung

Die Frontend-Version wird aus der Datei `package.json` übernommen.

Beispiel Entwicklungsstand:

```json
"version": "1.1.2-SNAPSHOT"
```

Beispiel Release:

```json
"version": "1.1.1"
```

Wichtiger Hinweis:

Die Versionsanzeige im Frontend wird zum Build-Zeitpunkt aus `package.json` erzeugt.

Vor jedem Release-Build muss die Version daher geprüft werden.

Nach Abschluss eines Releases wird die Entwicklungsversion auf die nächste Snapshot-Version angehoben.

---

### 4.3 Lokaler Build

Frontend-Projekt lokal unter Windows öffnen.

Build ausführen:

```bash
npm run build
```

Erwartetes Ergebnis:

```text
dist/
├── assets/
├── index.html
├── favicon.svg
└── icons.svg
```

Hinweis:

Für die nachfolgenden Docker-Schritte muss Docker Desktop gestartet sein.

Prüfung:

```powershell
docker version
```

---

### 4.4 Docker Image Build

Das Frontend verwendet unterschiedliche nginx-Konfigurationen:

```text
docker/nginx-local.conf
docker/nginx-dev.conf
docker/nginx-prod.conf
```

#### DEV Build

```bash
docker build \
  --platform linux/arm64 \
  --build-arg NGINX_CONF=nginx-dev.conf \
  -t emc-mitglieder-frontend:1.1.2-SNAPSHOT .
```

#### PROD Build

```bash
docker build \
  --platform linux/arm64 \
  --build-arg NGINX_CONF=nginx-prod.conf \
  -t emc-mitglieder-frontend:1.1.1 .
```

Wichtiger Hinweis:

Ohne die Plattformangabe

```text
--platform linux/arm64
```

kann das Image auf dem NAS nicht gestartet werden.

Typischer Fehler:

```text
exec /docker-entrypoint.sh: exec format error
```

---

### 4.5 Image Archivierung

Frontend Images werden lokal archiviert.

Verzeichnis:

```text
docker-image-archive/
```

Beispiel:

```text
docker-image-archive/
├── emc-mitglieder-frontend-1.1.1.tar
└── emc-mitglieder-frontend-1.1.1-SNAPSHOT.tar
```

Image exportieren:

#### DEV

```bash
docker save -o docker-image-archive/emc-mitglieder-frontend-1.1.2-SNAPSHOT.tar emc-mitglieder-frontend:1.1.2-SNAPSHOT
```

#### PROD

```bash
docker save -o docker-image-archive/emc-mitglieder-frontend-1.1.1.tar emc-mitglieder-frontend:1.1.1
```

Ziel:

- nachvollziehbare Deployment-Historie
- Rollback-Unterstützung
- reproduzierbare Deployments

---

### 4.6 Transfer auf NAS

Zielverzeichnisse:

DEV:

```text
/volume1/docker/build/mitglieder-frontend-dev/image
```

PROD:

```text
/volume1/docker/build/mitglieder-frontend-prod/image
```

#### DEV

```powershell
scp .\docker-image-archive\emc-mitglieder-frontend-1.1.2-SNAPSHOT.tar `
JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-frontend-dev/image/
```

#### PROD

```powershell
scp .\docker-image-archive\emc-mitglieder-frontend-1.1.1.tar `
JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-frontend-prod/image/
```

Übertragung prüfen:

```bash
ls -lh /volume1/docker/build/mitglieder-frontend-prod/image
```

Erwartet:

```text
emc-mitglieder-frontend-1.1.1.tar
```

---

### 4.7 Image Import auf NAS

#### DEV

```bash
docker load -i /volume1/docker/build/mitglieder-frontend-dev/image/emc-mitglieder-frontend-1.1.2-SNAPSHOT.tar
```

#### PROD

```bash
docker load -i /volume1/docker/build/mitglieder-frontend-prod/image/emc-mitglieder-frontend-1.1.1.tar
```

Kontrolle:

```bash
docker images emc-mitglieder-frontend
```

Erwartet:

```text
emc-mitglieder-frontend:1.1.2-SNAPSHOT
emc-mitglieder-frontend:1.1.1
```

---

### 4.8 Portainer Deployment

Portainer öffnen.

Stack auswählen:

```text
emc-mitglieder-frontend-dev
```

oder

```text
emc-mitglieder-frontend-prod
```

#### Deployment-Konfiguration prüfen

Vor jedem Redeploy muss geprüft werden, dass die gewünschte Image-Version sowohl in der Compose-Datei als auch im Portainer Stack eingetragen ist.

Quelle:

```text
/volume1/docker/compose/emc-mitglieder-frontend-dev/docker-compose.yml

/volume1/docker/compose/emc-mitglieder-frontend-prod/docker-compose.yml
```

Beispiel DEV:

```yaml
image: emc-mitglieder-frontend:1.1.2-SNAPSHOT
```

Beispiel PROD:

```yaml
image: emc-mitglieder-frontend:1.1.1
```

Zusätzlich prüfen:

Portainer
→ Stack
→ Editor

EMC-INFRA
→ compose
→ emc-mitglieder-frontend-dev oder emc-mitglieder-frontend-prod
→ docker-compose.yml

Bei Versionswechseln ist die Image-Version in

- EMC-INFRA
- der NAS-Compose-Datei
- der Portainer Stack Definition

konsistent anzupassen.

Erst danach darf ein Redeploy durchgeführt werden.

Kontrolle:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}"
```

Erwartet:

```text
emc-mitglieder-frontend-dev    emc-mitglieder-frontend:1.1.2-SNAPSHOT
emc-mitglieder-frontend-prod   emc-mitglieder-frontend:1.1.1
```

Aktion:

```text
Redeploy
```

Nach dem Redeploy prüfen:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}" | grep frontend
```

Optional:

```bash
docker logs emc-mitglieder-frontend-prod --tail 30
```

---

### 4.9 Frontend Smoke Test

Nach Deployment prüfen:

- Loginseite lädt
- CSS wird korrekt geladen
- JavaScript wird korrekt geladen
- Navigation funktioniert
- API Kommunikation funktioniert
- Login funktioniert
- Mitgliederliste lädt
- Frontend-Version korrekt
- Backend-Version korrekt

Zusätzlich:

Browserseite einmal neu laden.

Grund:

Der Browser kann JavaScript-Bundles aus dem Cache verwenden.

---

### 4.10 Typische Frontend Fehler

#### Falsche Versionsanzeige

Mögliche Ursache:

```text
package.json enthält noch eine Snapshot-Version
```

Prüfung:

```json
"version": "1.1.1"
```

---

#### Container startet nicht

Prüfung:

```bash
docker logs emc-mitglieder-frontend-prod
```

---

#### exec format error

Mögliche Ursache:

```text
Image nicht für linux/arm64 gebaut
```

Typischer Fehler:

```text
exec /docker-entrypoint.sh: exec format error
```

---

#### Falsche Backend-Umgebung

Mögliche Ursache:

```text
falsche nginx-Konfiguration verwendet
```

Prüfen:

```text
nginx-dev.conf
nginx-prod.conf
```

---

#### Alte Version sichtbar

Mögliche Ursache:

```text
Browser Cache
```

Maßnahme:

```text
Seite neu laden
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
→ Docker Image Build
→ Deployment-Konfiguration prüfen
→ Portainer Backend Stack Redeploy
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
  mv "$f" "/volume1/docker/build/archive/backend/${BASENAME}-dev-${TIMESTAMP}.jar"
done
```

PROD:

```bash
for f in /volume1/docker/build/mitglieder-backend-prod/target/*.jar; do
  [ -e "$f" ] || continue
  BASENAME=$(basename "$f" .jar)
  mv "$f" "/volume1/docker/build/archive/backend/${BASENAME}-prod-${TIMESTAMP}.jar"
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

> [!NOTE]
> Die folgenden SCP-Beispiele setzen voraus, dass die lokale PowerShell im Backend-Projektverzeichnis geöffnet ist.
>
> Beispiel:
>
> ```text
> C:\Users\Joerg\IdeaProjects\mitglieder-backend
> ```
>
> Andernfalls müssen absolute oder angepasste lokale Pfade verwendet werden.

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

#### Deployment-Konfiguration prüfen

Vor jedem Redeploy muss geprüft werden, dass die gewünschte Image-Version sowohl in der Compose-Datei als auch im Portainer Stack eingetragen ist.

Zusätzlich prüfen:

Portainer
→ Stack
→ Editor

EMC-INFRA
→ compose
→ emc-mitglieder-backend-dev oder emc-mitglieder-backend-prod
→ docker-compose.yml

NAS
→ /volume1/docker/compose
→ emc-mitglieder-backend-dev oder emc-mitglieder-backend-prod
→ docker-compose.yml

Die dort hinterlegte Compose-Konfiguration muss identisch sein.

Bei Versionswechseln ist die Image-Version in

- EMC-INFRA
- der NAS-Compose-Datei
- der Portainer Stack Definition

konsistent anzupassen.

Erst danach darf ein Redeploy durchgeführt werden.

Kontrolle:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}"
```

---

### 5.7 Portainer Deployment

Voraussetzung:

- gewünschtes Docker Image vorhanden
- Compose-Dateien aktualisiert
- Portainer Stack Definition aktualisiert

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
- `/actuator/health` liefert Status UP
- `/actuator/health/readiness` liefert Status UP
- `/api/system/info` liefert korrekte Version
- `/api/system/info` liefert korrekte Umgebung (DEV/PROD)

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

#### Falsche Version läuft

Möglich Ursache

- docker-compose.yml referenziert eine andere Image-Version

Prüfen:

- EMC-INFRA
- NAS Compose
- Portainer Stack
- Docker Image

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
→ relevante Tests erfolgreich
→ vollständige Testsuite erfolgreich
→ Projektdokumentation aktualisieren
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

2. Tests durchführen

3. Projektdokumentation aktualisieren

4. package.json Version prüfen

5. Frontend Build:

```bash
npm run build
```

6. Docker Image Build (linux/arm64)

7. Docker Image archivieren

8. DEV Deployment

9. DEV Smoke Test

10. PROD Deployment

11. PROD Smoke Test

12. Entwicklungsversion auf nächste Snapshot-Version anheben

---

### 7.3 Backend Release Ablauf

1. lokale Änderungen abschließen

2. relevante Backend Tests durchführen

3. vollständige Testsuite erfolgreich

4. Projektdokumentation aktualisieren

5. Backend Build:

```bash
mvn clean package
```

6. Deployment-Konfiguration prüfen

7. DEV Deployment

8. DEV Smoke Test

9. PROD Deployment

10. PROD Smoke Test

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
- `/actuator/health` liefert HTTP 200
- `/actuator/health/readiness` liefert HTTP 200
- `/api/system/info` liefert erwartete Backend-Version
- `/api/system/info` liefert erwartete Umgebung

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
- Backend DEV / PROD Health
- Backend DEV / PROD Readiness
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

Zusätzlich prüfen:

Backend SystemInfo:

```json
{
  "backendVersion": "...",
  "environment": "...",
  "activeProfiles": [...]
}
```

Erwartungen:

DEV:

```text
environment = DEV
activeProfiles = [dev]
```

PROD:

```text
environment = PROD
activeProfiles = [prod]
```

Zweck:

- schneller technischer Deployment-Sanity-Check
- Erkennung falscher Builds
- Erkennung fehlender Redeployments
- Erkennung von Frontend-/Backend-Versionsabweichungen
- Verifikation der Zielumgebung
- Erkennung fehlerhafter Runtime-Konfiguration
- Nachweis der korrekten Spring-Profil-Aktivierung

Wenn Versionen nicht erwartungsgemäß erscheinen:

- falscher Build deployed
- Container nicht redeployed
- Browser Cache aktiv
- falsche Zielumgebung verwendet

> [!NOTE]
> API-Tests gegen DEV und PROD erfolgen bewusst über den jeweiligen Frontend/nginx-Proxy.
>
> Das Backend ist aus Sicherheitsgründen nicht direkt exponiert.
>
> API-Testpfad:

```text
Postman
→ Frontend/nginx Proxy
→ Backend API
```

> Für API-Tests muss daher der Frontend-Container laufen und der `/api` Proxy korrekt funktionieren.
>
> Die React-Benutzeroberfläche selbst ist dafür nicht relevant.

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

älteres Frontend-Image wiederherstellen

Beispiel:

docker load -i emc-mitglieder-frontend-<version>.tar

anschließend:

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

### 9.5 Aktuelle Archivstrategie

Deployment-Artefakte werden vor Ersetzung archiviert.

Frontend:

```text
/volume1/docker/build/archive/frontend
```

Backend:

```text
/volume1/docker/build/archive/backend
```

Zweck:

- Rollback-Unterstützung
- technische Nachvollziehbarkeit
- Artefakt-Historie

Offene spätere Optimierungen:

- Aufbewahrungsstrategie
- Rotation
- automatisierte Bereinigung

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
