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
      - [Anmeldung erfolgreich, aber Session wird nicht übernommen](#anmeldung-erfolgreich-aber-session-wird-nicht-übernommen)
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
    - [7.6 Referenz: Release 1.2.1 vom 04.11.2026](#76-referenz-release-121-vom-04112026)
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
      - [Rollback über archiviertes Docker-Image](#rollback-über-archiviertes-docker-image)
    - [9.3 Backend Rollback](#93-backend-rollback)
      - [Rollback über gesichertes Docker-Image (bevorzugte Methode)](#rollback-über-gesichertes-docker-image-bevorzugte-methode)
    - [9.4 Datenbank Rollback](#94-datenbank-rollback)
    - [9.5 Aktuelle Archivstrategie](#95-aktuelle-archivstrategie)
      - [9.5.1 Frontend](#951-frontend)
      - [9.5.2 Backend](#952-backend)
      - [9.5.3 Allgemeine Archivierungsregeln](#953-allgemeine-archivierungsregeln)
      - [9.5.4 Offene spätere Optimierungen](#954-offene-spätere-optimierungen)
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
└── emc-mitglieder-frontend-1.2.0.tar
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
- lokaler Release-Build mit `mvn clean verify`

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

Für DEV werden Snapshot-Versionen verwendet:

```json
"version": "1.2.0-SNAPSHOT"
```

Für PROD werden stabile Release-Versionen ohne Snapshot-Kennzeichnung verwendet:

```json
"version": "1.2.0"
```

Die Datei `package-lock.json` muss dieselbe Projektversion enthalten wie `package.json`.

**Versionsänderung für einen PROD-Release:**

Im lokalen Frontend-Projekt:

```powershell
npm version 1.2.0 --no-git-tag-version
```

Dieser Befehl aktualisiert die Versionsangaben in `package.json` und `package-lock.json`, ohne automatisch einen Git-Tag zu erstellen.

Die angegebene Version ist bei jedem Release entsprechend anzupassen.

**Wichtige Hinweise:**

- Die Versionsanzeige im Frontend wird zum Build-Zeitpunkt aus `package.json` erzeugt.
- Vor jedem Release-Build müssen beide Paketdateien kontrolliert werden.
- Die Anwendungsversion muss zum geplanten Release passen; der Docker-Image-Tag bleibt umgebungsbezogen (`dev` oder `prod`).
- Der Git-Release-Tag wird nach erfolgreicher Release-Vorbereitung gesondert erstellt.
- Nach Abschluss eines Releases wird die Entwicklungsversion bei Beginn des nächsten Entwicklungsstands auf die vorgesehene Snapshot-Version angehoben.

**Historischer Referenzstand nach BL-020 / Release vom 26.10.2026:**

| Umgebung | Frontend-Version |
| -------- | ---------------- |
| DEV      | `1.2.0-SNAPSHOT` |
| PROD     | `1.2.0`          |

**Aktueller dokumentierter Stand (04.11.2026):** PROD verwendet Frontend `1.2.1`. In DEV wird die Cache-Korrektur BUG-003 mit Frontend `1.2.2-SNAPSHOT` beobachtet. Die Backend-Release-Version in PROD ist `1.2.1`. DEV-Versionen sind vor jedem Deployment anhand der tatsächlich laufenden Anwendung zu prüfen.

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

Das Frontend verwendet umgebungsbezogene Docker-Image-Tags, unabhängig von der Anwendungsversion in `package.json`:

| Umgebung | Fester Image-Tag | Beispiel für Anwendungsversion |
| --- | --- | --- |
| DEV | `emc-mitglieder-frontend:dev` | `1.2.2-SNAPSHOT` (BUG-003-Teststand) |
| PROD | `emc-mitglieder-frontend:prod` | `1.2.1` |

Die nginx-Konfiguration wird beim Build umgebungsbezogen ausgewählt.

#### DEV Build

```bash
docker build \
  --platform linux/arm64 \
  --build-arg NGINX_CONF=nginx-dev.conf \
  -t emc-mitglieder-frontend:dev .
```

#### PROD Build

```bash
docker build \
  --platform linux/arm64 \
  --build-arg NGINX_CONF=nginx-prod.conf \
  -t emc-mitglieder-frontend:prod .
```

**Wichtig:** Der Tag `dev` oder `prod` bezeichnet die Zielumgebung, **nicht** die Anwendungsversion. Vor dem Build `package.json` und `package-lock.json` prüfen. Vor einem erneuten Build unter demselben Tag das bisherige Image sichern (Kapitel 4.5). Ohne `--platform linux/arm64` kann auf dem NAS ein `exec format error` auftreten.

---

### 4.5 Image Archivierung

Die lokal gebauten Images werden vor dem Transfer als TAR-Datei exportiert. **Die Dateinamen enthalten die tatsächliche Anwendungsversion und die Zielumgebung**, auch wenn der Docker-Tag fest ist.

Beispiel (Versionsnummer bei jedem Build anpassen):

```bash
docker save -o docker-image-archive/emc-mitglieder-frontend-1.2.2-SNAPSHOT-dev.tar emc-mitglieder-frontend:dev
docker save -o docker-image-archive/emc-mitglieder-frontend-1.2.1-prod.tar emc-mitglieder-frontend:prod
```

Vor einem neuen PROD-Build oder -Import muss das **bisherige** Image für einen Rollback gesichert werden. Ein bloßer zusätzlicher Docker-Tag ist keine unabhängige Dateisicherung.

Zentrale NAS-Archivablage:

```text
/volume1/docker/build/archive/frontend/
```

Auf dem NAS kann das aktuell vorhandene PROD-Image vor dem Import des neuen Images beispielsweise wie folgt gesichert werden (Version und Datum vorher feststellen):

```bash
docker save -o /volume1/docker/build/archive/frontend/emc-mitglieder-frontend-1.2.0-prod-2026-11-04.tar emc-mitglieder-frontend:prod
```

**Das ist ein Befehlsbeispiel, kein Nachweis einer tatsächlich vorhandenen Archivdatei.** Version und Dateiname müssen zum realen Altstand passen. Archivdatei auf Existenz und plausible Größe prüfen und nicht ohne Entscheidung löschen. Historisch wurde beim Release `v1.2.0` das Frontend `1.1.1` archiviert.

---

### 4.6 Transfer auf NAS

Zielverzeichnisse:

```text
/volume1/docker/build/mitglieder-frontend-dev/image
/volume1/docker/build/mitglieder-frontend-prod/image
```

Beispiele für PowerShell (Dateinamen dem tatsächlichen Build anpassen):

```powershell
scp .\docker-image-archive\emc-mitglieder-frontend-1.2.2-SNAPSHOT-dev.tar `
  JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-frontend-dev/image/

scp .\docker-image-archive\emc-mitglieder-frontend-1.2.1-prod.tar `
  JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-frontend-prod/image/
```

Übertragung und Dateigröße auf dem NAS prüfen.

---

### 4.7 Image Import auf NAS

**Vor dem PROD-Import zuerst das bisherige PROD-Image sichern (Kapitel 4.5).** Beim Laden eines TAR mit festem Tag wird die Zuordnung `:prod` beziehungsweise `:dev` auf das neue Image geändert; der laufende Container wird dadurch noch nicht ersetzt.

Beispiele:

```bash
docker load -i /volume1/docker/build/mitglieder-frontend-dev/image/emc-mitglieder-frontend-1.2.2-SNAPSHOT-dev.tar
docker load -i /volume1/docker/build/mitglieder-frontend-prod/image/emc-mitglieder-frontend-1.2.1-prod.tar
```

Kontrolle:

```bash
docker images emc-mitglieder-frontend
```

Erwartete Tags: `emc-mitglieder-frontend:dev` und `emc-mitglieder-frontend:prod`. Die tatsächliche Anwendungsversion ist anschließend in der laufenden Anwendung zu prüfen.

---

### 4.8 Portainer Deployment

Die Frontend-Stacks verwenden feste Tags:

```yaml
# DEV-Stack
image: emc-mitglieder-frontend:dev
```

```yaml
# PROD-Stack
image: emc-mitglieder-frontend:prod
```

Vor dem Redeploy die Übereinstimmung der Compose-Konfigurationen in **EMC-INFRA**, auf dem **NAS** (`/volume1/docker/compose/emc-mitglieder-frontend-dev/` bzw. `...-prod/`) und im **Portainer Stack Editor** prüfen.

**Bei einem reinen Anwendungsversionwechsel bleiben diese Image-Referenzen unverändert.** Nur wenn die Compose-Konfiguration selbst geändert wird, müssen alle drei Stellen angepasst und die Änderung in EMC-INFRA versioniert werden.

Nach dem Image-Import den passenden Stack (`emc-mitglieder-frontend-dev` oder `emc-mitglieder-frontend-prod`) in Portainer redeployen. **Sicherstellen, dass der Container tatsächlich neu erstellt wird** und nicht das alte Image weiterläuft.

Kontrolle:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}" | grep frontend
```

Der feste Tag allein beweist nicht, welche Version läuft. Anschließend über den Info-Dialog die tatsächliche Frontend-Version, die Backend-Version und die Umgebung prüfen; außerdem den fachlichen Smoke-Test durchführen.

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
- Laufzeitumgebung in der Kopfzeile stimmt mit der tatsächlich angesprochenen Backend-Umgebung überein (DEV/PROD)
- Info-Dialog ist für angemeldete Benutzer erreichbar und zeigt plausible System- und Versionsinformationen
- In der Kopfzeile erscheinen keine technischen Verbindungsdetails wie `localhost` oder Proxy-Angaben

**Hinweis zur Kopierfunktion (BL-019 / BL-026):** Unter HTTP kann das Kopieren aus dem Info-Dialog wegen Browser-Sicherheitsanforderungen scheitern. Die verbindliche Prüfung für DEV und PROD nach HTTPS-Einführung gehört zu BL-026; ein fehlgeschlagener HTTP-Kopierversuch darf nicht als erfolgreicher Test dokumentiert werden.

**Zusätzliche Smoke Tests für BL-020 Reporting:**

- Reporting-Funktion im Frontend erreichbar
- Auswahl der verfügbaren Berichte funktioniert
- Mitgliederverzeichnis kann als PDF erzeugt und geöffnet werden
- Mitgliederverzeichnis wird im Querformat dargestellt
- Geburtstagsliste kann als PDF erzeugt und geöffnet werden
- Geburtstagsliste wird im Hochformat dargestellt
- Altersangaben werden korrekt angezeigt, soweit das Geburtsdatum bekannt ist
- Runde Geburtstage werden in roter Schrift hervorgehoben
- Mitglieder ohne bekanntes Geburtsdatum werden entsprechend der fachlichen Festlegung dargestellt
- Berichte enthalten die erwarteten Mitgliederdaten

**Hinweis:**

Diese Prüfungen wurden bei der Produktivsetzung von Release `v1.2.0` am 26.10.2026 für die beiden PDF-Berichte erfolgreich durchgeführt.

Bei zukünftigen Änderungen an der Reporting-Funktionalität sind die betroffenen Berichte erneut fachlich zu prüfen.

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
"version": "1.2.0"
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

Der Backend-Build erfolgt lokal unter Windows im Backend-Projekt mit Maven.

**Verbindlicher Build vor einem Release:**

```powershell
mvn clean verify
```

Dieser Befehl führt den vollständigen Maven-Build einschließlich der im Projekt konfigurierten Tests und Prüfungen aus.

Ein Release darf nur fortgesetzt werden, wenn der Build erfolgreich abgeschlossen wurde.

**Erwartetes Ergebnis:**

```text
BUILD SUCCESS
```

Das ausführbare Spring-Boot-Artefakt wird im Verzeichnis `target/` erzeugt.

Beispiel für Release `v1.2.0`:

```text
target/mitglieder-backend-1.2.0.jar
```

**Prüfungen vor dem Transfer auf das NAS:**

- Maven-Build erfolgreich abgeschlossen
- vollständige Testsuite erfolgreich
- erwartete Backend-Version in `pom.xml` eingetragen
- ausführbares JAR im Verzeichnis `target/` vorhanden
- keine Verwechslung mit älteren Build-Artefakten

**Referenzstand Release v1.2.0 vom 26.10.2026:**

| Prüfung         | Ergebnis           |
| --------------- | ------------------ |
| Maven-Befehl    | `mvn clean verify` |
| Backend-Version | `1.2.0`            |
| Backend-Tests   | 99 erfolgreich     |
| Build           | Erfolgreich        |

**Hinweis:**

Für reine Entwicklungs-Builds kann weiterhin `mvn clean package` verwendet werden. Für die Freigabe eines PROD-Releases gilt jedoch `mvn clean verify` als Standard.

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

Nach erfolgreichem lokalem Maven-Build wird das erzeugte Backend-Artefakt auf das NAS übertragen.

Für PROD-Releases ist zuvor der vollständige Build einschließlich der konfigurierten Tests und Prüfungen auszuführen:

```powershell
mvn clean verify
```

Der Build muss erfolgreich abgeschlossen sein, bevor das JAR-Artefakt übertragen wird.

**Zielverzeichnisse:**

DEV:

```text
/volume1/docker/build/mitglieder-backend-dev/target
```

PROD:

```text
/volume1/docker/build/mitglieder-backend-prod/target
```

---

#### Vorheriges Artefakt prüfen

Vor dem Transfer wird kontrolliert, welche JAR-Datei aktuell im jeweiligen Build-Verzeichnis liegt.

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-dev/target
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-prod/target
```

Die vorhandene Anwendungsversion ist vor der Archivierung festzustellen.

---

#### Vorheriges Artefakt archivieren

Vor dem Ersetzen eines bestehenden Backend-Artefakts wird die bisherige JAR-Datei archiviert.

Archivverzeichnis sicherstellen:

```bash
mkdir -p /volume1/docker/build/archive/backend
```

Zeitstempel erzeugen:

```bash
TIMESTAMP=$(date +%Y-%m-%d_%H-%M)
```

**DEV:**

```bash
for f in /volume1/docker/build/mitglieder-backend-dev/target/*.jar; do
  [ -e "$f" ] || continue
  BASENAME=$(basename "$f" .jar)
  mv "$f" "/volume1/docker/build/archive/backend/${BASENAME}-dev-${TIMESTAMP}.jar"
done
```

**PROD:**

```bash
for f in /volume1/docker/build/mitglieder-backend-prod/target/*.jar; do
  [ -e "$f" ] || continue
  BASENAME=$(basename "$f" .jar)
  mv "$f" "/volume1/docker/build/archive/backend/${BASENAME}-prod-${TIMESTAMP}.jar"
done
```

**Beispiel für Release v1.2.0 vom 26.10.2026:**

Beim Backend-PROD-Deployment wurde das bisherige JAR der Version `1.1.2` unter folgendem Namen archiviert:

```text
/volume1/docker/build/archive/backend/mitglieder-backend-1.1.2-prod-2026-10-26_15-24.jar
```

**Archivierungsregeln:**

- Vorhandene JAR-Dateien vor dem Ersetzen archivieren.
- DEV- und PROD-Artefakte anhand des Dateinamens unterscheiden.
- Versionsnummer und Zeitstempel im Archivnamen erhalten.
- Archivierte Artefakte nicht ohne gesonderte Entscheidung löschen.
- Die Archivierung vor dem Transfer des neuen JAR abschließen.

Ziel:

- nachvollziehbare Deployment-Historie
- Rollback-Möglichkeit
- keine Verwechslung mit älteren Artefakten

---

#### Build-Verzeichnis bereinigen

Nach erfolgreicher Archivierung sicherstellen, dass keine alten JAR-Dateien im jeweiligen `target`-Verzeichnis vorhanden sind.

DEV:

```bash
rm -f /volume1/docker/build/mitglieder-backend-dev/target/*.jar
```

PROD:

```bash
rm -f /volume1/docker/build/mitglieder-backend-prod/target/*.jar
```

**Kontrolle:**

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-dev/target
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-prod/target
```

Vor dem Transfer darf sich keine alte JAR-Datei mehr im jeweiligen `target`-Verzeichnis befinden.

**Wichtig:**

Die Bereinigung darf erst erfolgen, nachdem die Archivierung erfolgreich durchgeführt und überprüft wurde.

---

#### Neues Artefakt übertragen

Die folgenden SCP-Beispiele setzen voraus, dass die lokale PowerShell im Backend-Projektverzeichnis geöffnet ist.

Beispiel:

```text
C:\Users\Joerg\IdeaProjects\mitglieder-backend
```

Andernfalls müssen absolute oder entsprechend angepasste lokale Pfade verwendet werden.

Vor der Übertragung muss geprüft werden, dass die vorgesehene JAR-Datei im lokalen Verzeichnis `target/` vorhanden ist.

**Referenzstand Release v1.2.0:**

| Umgebung | JAR-Datei                               |
| -------- | --------------------------------------- |
| DEV      | `mitglieder-backend-1.2.0-SNAPSHOT.jar` |
| PROD     | `mitglieder-backend-1.2.0.jar`          |

**DEV-Transfer (Windows PowerShell):**

```powershell
scp target/mitglieder-backend-1.2.0-SNAPSHOT.jar JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-backend-dev/target/
```

**PROD-Transfer (Windows PowerShell):**

```powershell
scp target/mitglieder-backend-1.2.0.jar JaitiNissi1968@NAS-DH2300-C64C:/volume1/docker/build/mitglieder-backend-prod/target/
```

**Wichtige Hinweise:**

- Die angegebenen Versionsnummern entsprechen dem Release-Stand vom 26.10.2026.
- Bei zukünftigen Deployments sind die Dateinamen entsprechend anzupassen.
- Beim Transfer soll die JAR-Datei ausdrücklich mit ihrem vollständigen Dateinamen angegeben werden.
- Auf Platzhalter wie `mitglieder-backend-*.jar` sollte verzichtet werden, damit nicht versehentlich mehrere Artefakte übertragen werden.
- DEV- und PROD-Artefakte dürfen nicht verwechselt werden.

**Übertragung kontrollieren:**

DEV:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-dev/target
```

PROD:

```bash
ls -lh /volume1/docker/build/mitglieder-backend-prod/target
```

Nach dem Transfer muss im jeweiligen `target`-Verzeichnis genau eine gültige JAR-Datei mit der vorgesehenen Anwendungsversion vorhanden sein.

---

#### Docker Build Bezug

Der anschließende Docker-Build verwendet die folgende Anweisung aus dem Backend-Dockerfile:

```dockerfile
COPY target/*.jar app.jar
```

Damit wird die vorhandene JAR-Datei unabhängig von ihrer Versionsnummer in das Docker-Image übernommen.

Beispiele:

```text
mitglieder-backend-1.2.0-SNAPSHOT.jar
mitglieder-backend-1.2.0.jar
```

**Voraussetzung:**

Im jeweiligen `target`-Verzeichnis darf sich zum Zeitpunkt des Docker-Builds genau eine gültige JAR-Datei befinden.

Andernfalls kann die Docker-Build-Anweisung fehlschlagen oder ein nicht vorgesehenes Artefakt verarbeitet werden.

**Abschließende Prüfung vor dem Docker-Build:**

- Richtige Zielumgebung (DEV oder PROD)
- Erwartete JAR-Version vorhanden
- Genau eine JAR-Datei im `target`-Verzeichnis
- Vorheriges Artefakt erfolgreich archiviert
- Keine alten oder versehentlich übertragenen JAR-Dateien vorhanden

Nach erfolgreicher Kontrolle wird der Docker-Build gemäß Kapitel 5.6 durchgeführt.

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

**Backend-Image-Tags und Anwendungsversionen**

Im Gegensatz zum Frontend verwendet das Backend derzeit feste Docker-Image-Tags für die jeweilige Umgebung:

| Umgebung | Docker-Image-Tag              |
| -------- | ----------------------------- |
| DEV      | `emc-mitglieder-backend:dev`  |
| PROD     | `emc-mitglieder-backend:prod` |

Die tatsächliche Backend-Anwendungsversion wird über die Maven-Projektversion (`pom.xml`) und die beim Build erzeugten Versionsinformationen bestimmt.

Beispiel nach Release `v1.2.0` vom 26.10.2026:

| Eigenschaft                    | Wert                          |
| ------------------------------ | ----------------------------- |
| Backend-Anwendungsversion PROD | `1.2.0`                       |
| Docker-Image-Tag PROD          | `emc-mitglieder-backend:prod` |
| Git-Release-Tag                | `v1.2.0`                      |

**Wichtige Konsequenzen:**

- Der Docker-Image-Tag `prod` allein lässt keine Rückschlüsse auf die tatsächlich enthaltene Anwendungsversion zu.
- Bei jedem neuen PROD-Build wird der Tag `prod` dem neu erzeugten Image zugeordnet.
- Vor einem neuen PROD-Build muss deshalb das bisherige Image für einen möglichen Rollback gesichert werden.
- Nach dem Deployment muss die tatsächliche Backend-Version über die Anwendung beziehungsweise `/api/system/info` kontrolliert werden.
- Die Compose-Konfiguration bleibt bei einem Versionswechsel unverändert, solange weiterhin der Image-Tag `prod` verwendet wird.

**Rollback-Hinweis:**

Das bisherige Backend-Image sollte vor einem neuen Build zusätzlich mit einem eindeutigen Versions-Tag gesichert werden.

Beim Release `v1.2.0` wurde das bisherige PROD-Image beispielsweise unter folgendem Tag erhalten:

```text
emc-mitglieder-backend:1.1.2
```

Damit kann im Fehlerfall das vorherige Image erneut als `prod` getaggt und der Backend-PROD-Stack neu bereitgestellt werden.

Die Wiederherstellung muss anschließend durch einen vollständigen Smoke Test überprüft werden.

#### Deployment-Konfiguration prüfen

Vor jedem Backend-Redeployment muss geprüft werden, ob die Docker-Compose-Konfigurationen und die Portainer-Stack-Definition übereinstimmen.

Das Backend verwendet feste Docker-Image-Tags:

| Umgebung | Docker-Image                  |
| -------- | ----------------------------- |
| DEV      | `emc-mitglieder-backend:dev`  |
| PROD     | `emc-mitglieder-backend:prod` |

**Verbindliche Konfigurationsstellen:**

1. EMC-INFRA (Git-Repository)
   - `compose/emc-mitglieder-backend-dev/docker-compose.yml`
   - `compose/emc-mitglieder-backend-prod/docker-compose.yml`

2. NAS
   - `/volume1/docker/compose/emc-mitglieder-backend-dev/docker-compose.yml`
   - `/volume1/docker/compose/emc-mitglieder-backend-prod/docker-compose.yml`

3. Portainer
   - Stack `emc-mitglieder-backend-dev` → Editor
   - Stack `emc-mitglieder-backend-prod` → Editor

**Regel für Backend-Versionswechsel:**

Bei einem regulären Backend-Versionswechsel bleibt der Docker-Image-Tag unverändert. Ein neuer Docker-Build ersetzt die Zuordnung des jeweiligen Tags zum neuen Image.

Die Compose-Dateien müssen deshalb bei einem reinen Anwendungsversionwechsel nicht angepasst werden.

Änderungen an der Compose-Konfiguration selbst müssen dagegen weiterhin an allen drei Stellen konsistent vorgenommen und im Repository EMC-INFRA versioniert werden.

**Kontrolle vor dem Redeployment:**

```bash
docker images emc-mitglieder-backend
```

**Kontrolle nach dem Redeployment:**

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
```

Anschließend muss die tatsächlich laufende Backend-Anwendungsversion über `/api/system/info` geprüft werden.

**Wichtig:**

Der Image-Tag `prod` ist kein Nachweis für die enthaltene Anwendungsversion. Entscheidend ist, welches Docker-Image dem Tag zugeordnet ist und ob der Container nach dem Build tatsächlich neu erstellt wurde.

---

### 5.7 Portainer Deployment

Voraussetzung:

- gewünschtes Docker-Image vorhanden
- Compose-Dateien in EMC-INFRA und auf dem NAS geprüft
- Portainer-Stack-Definition auf Übereinstimmung geprüft
- Konfigurationsänderungen, sofern erforderlich, an allen drei Stellen konsistent übernommen

Bei einem reinen Backend-Anwendungsversionwechsel bleiben die
Compose-Konfigurationen unverändert, da die festen Docker-Image-Tags
`dev` beziehungsweise `prod` verwendet werden.

Nach dem Docker-Build muss der betreffende Container beim Redeployment
tatsächlich neu erstellt werden.

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

**Mögliche Ursachen:**

- Falsche oder veraltete JAR-Datei beim Docker-Build verwendet.
- Docker-Image nach dem Transfer des neuen JAR nicht neu gebaut.
- Docker-Image mit dem falschen Tag erstellt.
- Portainer-Stack redeployed, ohne den Container tatsächlich neu zu erstellen.
- Falsche Zielumgebung (DEV/PROD) aktualisiert.

**Prüfung:**

1. Erwartete Backend-Version in der `pom.xml` kontrollieren.
2. JAR-Datei im jeweiligen NAS-Build-Verzeichnis prüfen.
3. Docker-Images kontrollieren:

   ```bash
   docker images emc-mitglieder-backend
   ```

4. Laufenden Container und verwendetes Image prüfen:

   ```bash
   docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
   ```

5. Tatsächlich laufende Backend-Version über `/api/system/info` kontrollieren.
6. Bei Abweichungen Docker-Image erneut bauen und den betreffenden Portainer-Stack mit Neuerstellung des Containers bereitstellen.

**Wichtig:**

Die Backend-Docker-Image-Tags `dev` und `prod` sind feste Umgebungsbezeichnungen und keine Anwendungsversionen.

Eine korrekte Image-Bezeichnung allein garantiert deshalb nicht, dass die erwartete Backend-Version ausgeführt wird.

Die Compose-Konfiguration muss bei einem reinen Backend-Versionswechsel normalerweise nicht angepasst werden.

---

#### Anmeldung erfolgreich, aber Session wird nicht übernommen

**Fehlerbild:**

- Login-Anfrage wird vom Backend erfolgreich verarbeitet.
- Anschließende API-Anfragen werden als nicht authentifiziert behandelt.
- Die Anwendung kehrt zur Anmeldeseite zurück oder kann die Benutzersitzung nicht wiederherstellen.
- Backend-Healthchecks können gleichzeitig vollständig erfolgreich sein.

**Mögliche Ursache:**

Das Session-Cookie `JSESSIONID` wird mit dem Attribut `Secure` ausgeliefert, obwohl der Browser die Anwendung über HTTP aufruft.

Browser übertragen Cookies mit dem Attribut `Secure` grundsätzlich nur über sichere HTTPS-Verbindungen.

**Aktuelle PROD-Konfiguration:**

Datei im Backend-Repository:

`src/main/resources/application-prod.properties`

```properties
server.servlet.session.cookie.secure=false
```

Diese Einstellung ist erforderlich, solange der produktive Browserzugriff auf die EMC Mitgliederverwaltung über HTTP erfolgt.

**Prüfung bei Anmeldeproblemen:**

1. Tatsächlich verwendetes Protokoll im Browser prüfen (`http://` oder `https://`).
2. Im Browser über die Entwicklertools das Session-Cookie `JSESSIONID` kontrollieren.
3. Prüfen, ob das Cookie bei nachfolgenden API-Anfragen übertragen wird.
4. Backend-Konfiguration für das aktive Spring-Profil kontrollieren.
5. Nach einer Konfigurationsänderung Backend neu bauen und deployen.
6. Anmeldung und Session-Wiederherstellung erneut testen.

**Sicherheitshinweis:**

Die Einstellung `Secure=false` stellt eine betriebsbedingte Ausnahme dar und ist keine allgemeine Sicherheitsempfehlung.

Bei HTTP-Verbindungen besteht kein transportseitiger Schutz der Sitzungsdaten. Der Zugriff muss daher auf ein vertrauenswürdiges internes Netzwerk beschränkt bleiben.

Bei einer späteren Umstellung auf HTTPS ist die Konfiguration erneut zu prüfen. Bei durchgehendem HTTPS-Browserzugriff muss `Secure=true` aktiviert werden.

**Historischer Bezug:**

Das Fehlerbild trat bei der Produktivsetzung des Backend-Releases `v1.2.0` am 26.10.2026 auf und wurde durch Anpassung der PROD-Konfiguration behoben.

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

**Stand 04.11.2026:** BL-027 hat 14 fachliche Datumsfelder in sieben MariaDB-Tabellen von `DATETIME` auf `DATE` umgestellt. Die Änderung wurde in TEST, DEV und PROD geprüft. Die Verarbeitung im Backend verwendet für diese fachlichen Datumsfelder `LocalDate`; die bestehende Access-Anbindung wurde auf Kompatibilität geprüft.

Aktuell existiert keine automatisierte Schema-Migration (z. B. Flyway/Liquibase).

Datenbankänderungen erfolgen kontrolliert und manuell.

> [!WARNING]
> Datenbankänderungen müssen besonders sorgfältig dokumentiert und geprüft werden.

---

## 7. DEV → PROD Release Prozess

Dieses Kapitel beschreibt den standardisierten Release-Ablauf.

---

### 7.1 Standardprozess

Der Release-Prozess erfolgt manuell und kontrolliert. DEV und PROD werden getrennt behandelt.

**Verbindlicher Ablauf:**

```text
Entwicklung abschließen
→ relevante Tests durchführen
→ vollständige Testsuite erfolgreich
→ Änderungen in Git sichern
→ DEV Deployment
→ technische und fachliche DEV-Prüfung
→ PROD-Release vorbereiten
→ bisherige PROD-Artefakte und Images sichern
→ PROD Deployment
→ technische und fachliche PROD-Smoke-Tests
→ Monitoring kontrollieren
→ Release-Versionen und Git-Tags prüfen
→ Infrastruktur- und Projektdokumentation abschließen
→ Änderungen committen und nach GitHub übertragen
```

**Regeln für gemeinsame Frontend-/Backend-Releases:**

- Backend und Frontend müssen vor dem PROD-Deployment in DEV gemeinsam erfolgreich getestet worden sein.
- Bei neuen Backend-Funktionen, die das Frontend benötigt, wird grundsätzlich zuerst das Backend und anschließend das Frontend produktiv gesetzt.
- Zwischen den Deployments muss die Kompatibilität der Komponenten berücksichtigt werden.
- Nach jedem einzelnen Deployment erfolgt eine technische Funktionsprüfung.
- Nach Abschluss beider Deployments erfolgt ein gemeinsamer fachlicher Smoke Test.

**Release-Versionierung:**

- DEV verwendet die vorgesehene Snapshot-Version.
- PROD verwendet eine stabile Release-Version.
- Backend und Frontend besitzen jeweils einen eigenen Git-Release-Tag.
- Git-Tags und Anwendungsversionen müssen übereinstimmen.
- Änderungen an Docker-Compose-Konfigurationen werden im Repository EMC-INFRA versioniert.

**Release-Abschluss:**

Ein Release gilt erst als abgeschlossen, wenn die produktiven Anwendungen erfolgreich geprüft wurden, das Monitoring unauffällig ist und die zugehörigen Git- und Dokumentationsänderungen nachvollziehbar gesichert sind.

**Referenz: Release v1.2.0 vom 26.10.2026**

Bei diesem Release wurde zuerst das Backend und anschließend das Frontend produktiv gesetzt. Beide Anwendungen wurden erfolgreich geprüft; die Versionsanzeigen zeigten jeweils `v1.2.0`. Die zugehörigen Git-Release-Tags wurden erstellt und nach GitHub übertragen.

---

### 7.2 Frontend Release Ablauf

Der Frontend-Release erfolgt nach erfolgreicher Entwicklung und Prüfung in DEV.

**Verbindlicher Ablauf:**

1. Änderungen im Frontend abschließen und relevante Tests durchführen.
2. Vollständige Frontend-Testsuite erfolgreich ausführen.
3. Änderungen in Git sichern und den DEV-Stand prüfen.
4. Frontend in DEV bereitstellen und technische sowie fachliche Smoke Tests durchführen.
5. PROD-Release-Version festlegen und `package.json` sowie `package-lock.json` synchronisieren:

   ```powershell
   npm version X.Y.Z --no-git-tag-version
   ```

6. Frontend mit der Release-Version erneut bauen:

   ```powershell
   npm run build
   ```

7. Docker-Image für `linux/arm64` mit `nginx-prod.conf` und dem festen Image-Tag `emc-mitglieder-frontend:prod` erstellen.
8. Bisheriges PROD-Docker-Image **vor Überschreiben des festen Tags `prod`** für einen möglichen Rollback archivieren.
9. Neues Docker-Image exportieren, auf das NAS übertragen und dort importieren.
10. Feste Image-Referenz `emc-mitglieder-frontend:prod` in EMC-INFRA, NAS-Compose-Datei und Portainer Stack Editor auf Übereinstimmung prüfen; bei reinem Versionswechsel keine Compose-Änderung.
11. Frontend-PROD-Stack in Portainer neu bereitstellen.
12. Technischen und fachlichen PROD-Smoke-Test durchführen.
13. Frontend-Version und Backend-Kompatibilität prüfen.
14. Monitoring in Uptime Kuma kontrollieren.
15. Release-Änderungen im Frontend-Repository committen, Git-Release-Tag `vX.Y.Z` erstellen und nach GitHub übertragen.
16. Änderungen an den Compose-Dateien im Repository EMC-INFRA committen und nach GitHub übertragen.
17. Changelog und Release-Dokumentation aktualisieren.

**Wichtige Hinweise:**

- Der PROD-Docker-Image-Tag bleibt `prod`; die Anwendungsversion in `package.json` muss der geplanten Release-Version entsprechen.
- Der Git-Release-Tag wird separat erstellt und nicht durch `npm version` erzeugt.
- Ein Frontend-Release darf nur mit einer kompatiblen Backend-Version produktiv gesetzt werden.
- Nach einem abgeschlossenen Release wird die nächste Snapshot-Version erst im Rahmen der weiteren Entwicklung vorbereitet.
- Die detaillierten technischen Befehle sind in Kapitel 4 dokumentiert.

**Referenzstand Release v1.2.0 vom 26.10.2026:**

| Eigenschaft             | Wert             |
| ----------------------- | ---------------- |
| Frontend PROD-Version   | `1.2.0`          |
| Frontend DEV-Version    | `1.2.0-SNAPSHOT` |
| Git-Release-Tag         | `v1.2.0`         |
| Vorherige PROD-Version  | `1.1.1`          |
| Frontend-Release-Commit | `30aa8a8`        |
| EMC-INFRA-Commit        | `164ab02`        |

---

### 7.3 Backend Release Ablauf

Der Backend-Release erfolgt nach erfolgreicher Entwicklung und Prüfung in DEV.

**Verbindlicher Ablauf:**

1. Änderungen im Backend abschließen und relevante Tests durchführen.
2. Vollständige Backend-Testsuite erfolgreich ausführen:

   ```powershell
   mvn clean verify
   ```

3. Änderungen in Git sichern und den DEV-Stand prüfen.
4. Backend in DEV bereitstellen und technische sowie fachliche Smoke Tests durchführen.
5. PROD-Release-Version in der `pom.xml` festlegen und kontrollieren.
6. Backend mit der PROD-Release-Version erneut bauen:

   ```powershell
   mvn clean verify
   ```

7. Vorhandenes PROD-JAR auf dem NAS archivieren.
8. Bisheriges PROD-Docker-Image mit einem eindeutigen Versions-Tag für einen möglichen Rollback sichern.
9. Neues Backend-JAR auf das NAS übertragen.
10. Sicherstellen, dass im PROD-Build-Verzeichnis nur das vorgesehene JAR vorhanden ist.
11. Neues Backend-Docker-Image mit dem festen Tag `prod` erstellen:

    ```bash
    cd /volume1/docker/build/mitglieder-backend-prod
    docker build -t emc-mitglieder-backend:prod .
    ```

12. Backend-Compose-Konfigurationen in EMC-INFRA, auf dem NAS und in Portainer auf Konsistenz prüfen.
13. Backend-PROD-Stack in Portainer neu bereitstellen.
14. Technischen und fachlichen PROD-Smoke-Test durchführen.
15. Tatsächlich laufende Backend-Version und Umgebung über `/api/system/info` prüfen.
16. Backend-Healthchecks und Readiness sowie Monitoring in Uptime Kuma kontrollieren.
17. Release-Änderungen im Backend-Repository committen, Git-Release-Tag `vX.Y.Z` erstellen und nach GitHub übertragen.
18. Changelog und Release-Dokumentation aktualisieren.

**Wichtige Hinweise:**

- Das Backend verwendet die festen Docker-Image-Tags `dev` und `prod`.
- Bei einem reinen Backend-Anwendungsversionwechsel müssen die Compose-Dateien nicht angepasst werden.
- Ein neuer Docker-Build ersetzt die Zuordnung des Tags `prod` zum neu erzeugten Image.
- Der bisherige PROD-Image-Stand muss deshalb vor dem Build für einen möglichen Rollback gesichert werden.
- Die tatsächliche Anwendungsversion ist nach dem Deployment über `/api/system/info` zu kontrollieren.
- Bei gemeinsamen Backend-/Frontend-Releases mit neuen API-Funktionen wird grundsätzlich zuerst das Backend produktiv gesetzt.
- Die detaillierten technischen Befehle sind in Kapitel 5 dokumentiert.

**Referenzstand Release v1.2.0 vom 26.10.2026:**

| Eigenschaft                | Wert                           |
| -------------------------- | ------------------------------ |
| Backend PROD-Version       | `1.2.0`                        |
| Backend DEV-Version        | `1.2.0-SNAPSHOT`               |
| Git-Release-Tag            | `v1.2.0`                       |
| Vorherige PROD-Version     | `1.1.2`                        |
| Gesichertes Rollback-Image | `emc-mitglieder-backend:1.1.2` |
| Backend-Release-Commit     | `2f6fa66`                      |
| Maven-Tests                | 99 erfolgreich                 |

**Besonderheit beim Release v1.2.0:**

Nach dem ersten PROD-Deployment wurde festgestellt, dass die Anmeldung aufgrund des `Secure`-Attributs des Session-Cookies bei HTTP-Zugriff nicht dauerhaft übernommen wurde.

Die Einstellung `server.servlet.session.cookie.secure` wurde für die bestehende HTTP-Betriebsumgebung auf `false` gesetzt. Anschließend wurde das Backend erneut gebaut und erfolgreich bereitgestellt.

Die technische Fehlerbeschreibung und der Sicherheitshinweis sind in Kapitel 5.9 dokumentiert.

---

### 7.4 Release mit Datenbankänderungen

**Zusätzlicher Ablauf:**

1. Betroffene Tabellen und Felder sowie Auswirkungen auf Backend, Frontend und Access ermitteln.
2. Datenbankänderung zunächst in TEST beziehungsweise DEV erproben und Datenintegrität prüfen.
3. Betroffene Anwendungsversionen und die bestehende Access-Anbindung in DEV testen.
4. Vor PROD-Migration ein geeignetes Datenbankbackup erstellen und dessen Vorhandensein prüfen.
5. Reihenfolge von Datenbankänderung und Anwendungsdeployments anhand der Kompatibilität festlegen; die Reihenfolge ist nicht für jede Migration identisch.
6. PROD-Datenbankänderung kontrolliert durchführen und Ergebnis prüfen.
7. Backend und gegebenenfalls Frontend bereitstellen; technische und fachliche Smoke-Tests einschließlich Schreib- und Persistenzprüfung durchführen.
8. Access-Kompatibilität erneut prüfen und die tatsächlich durchgeführten Schritte im Ergebnisdokument festhalten.

**Rollback-Grenze:** Ein Rollback von Backend oder Frontend stellt ein geändertes Datenbankschema nicht wieder her. Besonders nach Release `1.2.1` (Umstellung `DATETIME` → `DATE`) ist die Kompatibilität einer älteren Anwendungsversion mit dem aktuellen Schema vor einem Rollback zu klären. Ein Datenbank-Restore ist eine separate Entscheidung; dabei können zwischenzeitliche Änderungen verloren gehen.

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

### 7.6 Referenz: Release 1.2.1 vom 04.11.2026

Am 04.11.2026 wurden Frontend und Backend in Version `1.2.1` produktiv bereitgestellt. Das Release umfasst BL-019 (Anzeige der tatsächlichen Backend-Laufzeitumgebung und Info-Dialog) sowie BL-027 (Harmonisierung fachlicher Datumsfelder).

- BL-027: 14 Felder in sieben MariaDB-Tabellen von `DATETIME` auf `DATE` umgestellt; vorheriges PROD-Datenbankbackup und Access-Kompatibilitätsprüfung dokumentiert.
- Qualitätssicherung: 99 Backend-Tests und 65 Frontend-Tests erfolgreich.
- BL-019: Frontend-Release-Commit `6faaa61`; die Kopierfunktion ist unter HTTP browserabhängig eingeschränkt, Nachprüfung unter HTTPS in BL-026.
- Historische Befehlsbeispiele für `1.2.0` in diesem Handbuch bleiben als Beispiele erhalten und sind **nicht** als aktuelle Image-/JAR-Version zu verwenden.

**Quellen:** `BL-019-frontend-anzeige-laufzeitumgebung-ergebnis.md` und `BL-027-umsetzung-ergebnis-v0.3.md` in den jeweiligen Anforderungsordnern sowie `CHANGELOG.md`.

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

**Zusätzliche Prüfung bei Reporting-Änderungen:**

- Betroffene Berichte im Frontend auswählen und erzeugen
- PDF-Ausgabe auf Vollständigkeit und fachliche Richtigkeit prüfen
- Berichtsabhängige Berechnungen und Hervorhebungen kontrollieren

Die detaillierten Prüfungen für die mit BL-020 eingeführten Berichte sind in Kapitel 4.9 dokumentiert.

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

Nach Deployment Versionsanzeige und tatsächliche Backend-Laufzeitumgebung prüfen. Die Benutzeroberfläche zeigt die Umgebung in der Kopfzeile; weitere Versions- und Systeminformationen sind im Info-Dialog zugänglich. Die technische Gegenprüfung erfolgt über `/api/system/info`.

Frontend:

- Frontend-Version sichtbar
- erwartete Version korrekt

Backend:

- Backend-Version sichtbar
- erwartete Version korrekt

Beispiel:

```text
PROD: FE v1.2.0 | BE v1.2.0
DEV:  FE v1.2.0-SNAPSHOT | BE v1.2.0-SNAPSHOT
```

**Historischer Referenzstand** nach dem Release vom 26.10.2026. Aktueller PROD-Stand seit 04.11.2026: `FE v1.2.1 | BE v1.2.1`. Das Frontend in DEV verwendet im Rahmen von BUG-003 inzwischen `1.2.2-SNAPSHOT`; die tatsächlich laufende Backend-Version ist gesondert zu prüfen. Die erwarteten Versionsnummern sind bei jedem zukünftigen Release entsprechend anzupassen.

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

Ein Rollback dient dazu, nach einem fehlgeschlagenen Deployment eine zuvor funktionsfähige Anwendungsversion wiederherzustellen.

Rollback-Maßnahmen erfolgen derzeit manuell und kontrolliert über Docker und Portainer.

**Bevorzugte Wiederherstellungsverfahren:**

| Komponente | Bevorzugtes Verfahren                                                                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------- |
| Frontend   | Vorheriges Docker-Image laden, wieder dem festen Tag `prod` zuordnen und Container neu erstellen |
| Backend    | Gesichertes Docker-Image erneut mit dem festen Tag `prod` versehen und Stack neu bereitstellen                |
| Datenbank  | Gesonderte Prüfung und gegebenenfalls Wiederherstellung aus einem geeigneten Backup                           |

**Verbindliche Grundregeln:**

- Vor jedem PROD-Deployment die bisherige Anwendungsversion feststellen.
- Vorherige PROD-Artefakte beziehungsweise Docker-Images für einen möglichen Rollback sichern.
- Vor einem Rollback die Kompatibilität zwischen Frontend, Backend und Datenbankschema prüfen.
- Keine Änderungen direkt in laufenden Containern durchführen.
- Die Wiederherstellung erfolgt über die dokumentierten Docker- und Portainer-Verfahren.
- Nach einem Rollback die tatsächlich laufenden Anwendungsversionen kontrollieren.
- Technische und fachliche Smoke Tests durchführen.
- Monitoring und Logs prüfen.
- Rollback-Maßnahmen einschließlich Ursache und Ergebnis dokumentieren.

**Wichtig:**

Ein Docker-Rollback stellt zunächst nur die Anwendungskomponente wieder her. Bereits durchgeführte Datenbankänderungen werden dadurch nicht rückgängig gemacht. **Seit Release `1.2.1` ist insbesondere die `DATE`-Schemaänderung aus BL-027 zu beachten:** Ein Rücksprung auf `1.2.0` ist nicht ohne vorherige Kompatibilitätsprüfung freizugeben.

Ein Datenbank-Restore ist deshalb gesondert zu bewerten und darf nicht automatisch als Bestandteil eines Anwendungs-Rollbacks erfolgen.

**Alternative Wiederherstellung:**

Falls ein zuvor gesichertes Docker-Image nicht verfügbar ist, kann ein archiviertes Backend-JAR verwendet werden, um das entsprechende Docker-Image erneut zu erstellen.

Die komponentenspezifischen Verfahren sind in Kapitel 9.2 bis 9.4 beschrieben.

---

### 9.2 Frontend Rollback

Das Frontend verwendet feste Image-Tags `dev` und `prod`. Ein Rollback setzt deshalb **nicht** die Compose-Image-Referenz auf eine Versionsnummer zurück. Stattdessen wird ein zuvor gesichertes Image wieder dem Tag `prod` zugeordnet.

#### Rollback über archiviertes Docker-Image

1. Vorherige Anwendungsversion und vorhandene Archivdatei ermitteln. Archivierte TAR-Datei und tatsächliche Version müssen übereinstimmen.
2. **Vor dem Zurücksetzen die Kompatibilität mit der aktuellen Backend-Version und dem Datenbankschema prüfen.** Insbesondere nach BL-027 (`DATETIME` → `DATE`) ist ein Rücksprung auf `1.2.0` nicht automatisch freigegeben.
3. Das Archivimage laden. Beispiel mit **Platzhalter**:

   ```bash
   docker load -i /volume1/docker/build/archive/frontend/<archivdatei>.tar
   ```

4. Geladenes Image ermitteln (`docker images`) und gegebenenfalls dem festen PROD-Tag zuordnen:

   ```bash
   docker tag <gesichertes-image-id-oder-tag> emc-mitglieder-frontend:prod
   ```

   Falls das Archiv bereits den Tag `:prod` enthält, ist eine zusätzliche Zuordnung nicht erforderlich. Den neu zugeordneten Image-Inhalt vor dem Redeploy kontrollieren.

5. Prüfen, dass EMC-INFRA, NAS-Compose und Portainer weiterhin `image: emc-mitglieder-frontend:prod` verwenden. **Keinen Versions-Tag in Compose eintragen.**
6. Frontend-PROD-Stack in Portainer mit **Neuerstellung des Containers** redeployen.
7. Laufende Anwendungsversion über den Info-Dialog kontrollieren; Anmeldung, Navigation, API-Kommunikation und betroffene Funktionen testen. Monitoring prüfen und Rollback dokumentieren.

**Wichtig:** `docker load` und `docker tag` ersetzen keinen laufenden Container. Ein Rollback der Anwendung setzt auch keine Datenbankänderung zurück.

---

### 9.3 Backend Rollback

#### Rollback über gesichertes Docker-Image (bevorzugte Methode)

Vor einem PROD-Release wird das bisherige Backend-Image mit einem eindeutigen Versions-Tag gesichert.

Beispiel für Release `v1.2.0` vom 26.10.2026:

| Eigenschaft               | Wert                           |
| ------------------------- | ------------------------------ |
| Neue Backend-Version      | `1.2.0`                        |
| Vorherige Backend-Version | `1.1.2`                        |
| Aktiver PROD-Image-Tag    | `emc-mitglieder-backend:prod`  |
| Gesicherter Rollback-Tag  | `emc-mitglieder-backend:1.1.2` |

**Rollback-Ablauf auf dem NAS:**

1. Vorhandenes Rollback-Image prüfen:

```bash
docker images emc-mitglieder-backend
```

2. Vorherige Version wieder mit dem PROD-Tag versehen:

```bash
docker tag emc-mitglieder-backend:1.1.2 emc-mitglieder-backend:prod
```

3. Im Portainer den Stack `emc-mitglieder-backend-prod` neu bereitstellen.

Dabei sicherstellen, dass der Container tatsächlich aus dem wiederhergestellten Image neu erstellt wird.

4. Nach dem Redeployment prüfen:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
```

5. Anschließend die tatsächliche Backend-Version über `/api/system/info` kontrollieren und den vollständigen Backend-Smoke-Test durchführen.

**Wichtig:**

Der Befehl `docker tag` verändert lediglich die Image-Zuordnung. Er startet oder ersetzt keinen laufenden Container.

Ein erfolgreicher Rollback ist erst bestätigt, wenn der neu erstellte Container die erwartete ältere Anwendungsversion ausführt und die Funktionstests erfolgreich sind.

**Voraussetzung:**

Diese Methode ist nur geeignet, wenn die vorherige Anwendungsversion mit dem aktuellen Datenbankschema und den übrigen Komponenten kompatibel ist.

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

Vor einem produktiven Deployment werden die bisherigen Anwendungsartefakte gesichert, um bei Bedarf eine zuvor funktionsfähige Version wiederherstellen zu können.

Die Archivierung erfolgt derzeit manuell. Eine automatische Rotation oder Bereinigung ist nicht eingerichtet.

#### 9.5.1 Frontend

Das bisherige PROD-Docker-Image wird vor dem Deployment als TAR-Datei exportiert und auf dem NAS archiviert.

**Archivverzeichnis:**

```text
/volume1/docker/build/archive/frontend
```

**Beispiel aus dem Release v1.2.0 vom 26.10.2026:**

```text
emc-mitglieder-frontend-1.1.1-prod-2026-10-26.tar
```

Die archivierte Datei enthält das bisherige Frontend-Docker-Image und kann bei einem Rollback mit `docker load` wiederhergestellt werden.

Die Frontend-Images werden inzwischen mit festen Umgebungstags betrieben, beispielsweise:

```text
emc-mitglieder-frontend:prod
```

Bei einem Rollback wird das gesicherte ältere Image wieder dem festen Tag `prod` zugeordnet und der Container neu erstellt. Die Compose-Image-Referenz bleibt dabei unverändert.

Das konkrete Wiederherstellungsverfahren ist in Kapitel 9.2 beschrieben.

#### 9.5.2 Backend

Für das Backend werden zwei Sicherungsmechanismen verwendet.

**1. Sicherung des bisherigen Docker-Images**

Vor dem PROD-Deployment wird das bisherige Docker-Image zusätzlich mit einem versionsbezogenen Tag versehen.

Beispiel aus dem Release v1.2.0:

```bash
docker tag emc-mitglieder-backend:prod emc-mitglieder-backend:1.1.2
```

Der produktive Backend-Stack verwendet weiterhin den festen Image-Tag:

```text
emc-mitglieder-backend:prod
```

Bei einem Rollback kann das gesicherte Image erneut mit `prod` getaggt und der Stack neu bereitgestellt werden.

**Wichtig:** Die Sicherung über einen zusätzlichen Docker-Tag ist keine unabhängige Dateiarchivierung. Sie setzt voraus, dass das entsprechende Docker-Image auf dem NAS weiterhin vorhanden ist und nicht durch eine Image-Bereinigung entfernt wurde.

**2. Archivierung der bisherigen JAR-Datei**

Zusätzlich wird die bisherige Backend-JAR-Datei vor ihrer Ersetzung archiviert.

**Archivverzeichnis:**

```text
/volume1/docker/build/archive/backend
```

**Beispiel aus dem Release v1.2.0 vom 26.10.2026:**

```text
mitglieder-backend-1.1.2-prod-2026-10-26_15-24.jar
```

Die archivierte JAR-Datei ermöglicht bei Bedarf den erneuten Aufbau eines Backend-Docker-Images.

Das Verfahren zur Archivierung und Übertragung der Backend-JAR-Dateien ist in Kapitel 5.5 dokumentiert.

Das konkrete Backend-Rollback-Verfahren ist in Kapitel 9.3 beschrieben.

#### 9.5.3 Allgemeine Archivierungsregeln

- Vor jedem PROD-Deployment die bisherige Anwendungsversion feststellen.
- Vorhandene Artefakte vor ihrer Ersetzung sichern.
- Archivdateien mit Versionsnummer und gegebenenfalls Zeitstempel kennzeichnen.
- DEV- und PROD-Artefakte eindeutig unterscheiden.
- Erfolgreiche Archivierung vor dem Deployment kontrollieren.
- Archivierte Artefakte nicht ohne gesonderte Entscheidung löschen.
- Sicherstellen, dass für einen geplanten Rollback die erforderlichen Artefakte tatsächlich verfügbar sind.

Die Archivierung dient folgenden Zwecken:

- Unterstützung eines kontrollierten Rollbacks
- Technische Nachvollziehbarkeit von Deployments
- Erhaltung der Artefakt-Historie
- Wiederherstellung früherer Anwendungsstände

#### 9.5.4 Offene spätere Optimierungen

Folgende Verbesserungen sind derzeit noch nicht umgesetzt:

- Festlegung einer verbindlichen Aufbewahrungsdauer
- Automatische Archivrotation
- Automatisierte Bereinigung veralteter Artefakte
- Prüfung einer zusätzlichen dateibasierten Archivierung von Backend-Docker-Images
- Regelmäßige Überprüfung der Wiederherstellbarkeit archivierter Artefakte

Bis zur Einführung entsprechender Verfahren erfolgt die Archivverwaltung manuell.

Eine automatische Löschung archivierter Artefakte ist derzeit nicht vorgesehen.

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
