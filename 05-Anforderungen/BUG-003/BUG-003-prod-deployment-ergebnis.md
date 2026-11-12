# BUG-003 – Frontend 1.2.3 – PROD-Deployment und Prüfergebnis

**Datum:** 12.11.2026  
**Projekt:** EMC Mitgliederverwaltung  
**Komponente:** Frontend (React/Vite, Nginx)  
**Zielumgebung:** PROD  
**Release-Version:** 1.2.3  
**Status:** Erfolgreich bereitgestellt und abgenommen  
**Zugehöriger Fehler:** BUG-003 – Veraltete Frontend-Version durch Browser-Cache

---

## 1. Anlass und Ziel

Am 12.11.2026 wurde die Frontend-Version `1.2.3` in der Produktionsumgebung der EMC Mitgliederverwaltung bereitgestellt.

Das Release enthält insbesondere:

- Die Korrektur der Browser-Cache-Behandlung für `index.html` im Rahmen von BUG-003.
- Die Aktualisierung der Frontend-Abhängigkeiten zur Behebung bekannter Sicherheitslücken in Produktionsabhängigkeiten.

Die Korrektur war zuvor mit `1.2.2-SNAPSHOT` erstmals in DEV bereitgestellt und anschließend mit `1.2.3-SNAPSHOT` weiter getestet worden.

Die DEV-Umgebung lief vom 09.11.2026 bis zum 12.11.2026 stabil. Ein erneutes Auftreten der weißen Browserseite wurde nicht gemeldet.

Auf dieser Grundlage wurde das PROD-Release freigegeben.

Die Version `1.2.2` wurde nicht als eigenständiges PROD-Release veröffentlicht.

---

## 2. Release-Vorbereitung und Qualitätssicherung

### 2.1 Versionsanhebung

Die Frontend-Version wurde von `1.2.3-SNAPSHOT` auf `1.2.3` geändert.

Betroffene Dateien:

- `package.json`
- `package-lock.json`

Weitere Quellcodeänderungen waren für die Versionsanhebung nicht erforderlich.

### 2.2 Qualitätssicherung

Vor dem Release wurden folgende Prüfungen durchgeführt:

| Prüfung | Ergebnis |
|---|---|
| `npm ci` | Erfolgreich |
| Frontend-Unit-Tests | 65 von 65 erfolgreich |
| Testdateien | 7 von 7 erfolgreich |
| `npm run build` | Erfolgreich |
| Vite-Version | 6.4.6 |
| Vitest-Version | 3.2.7 |
| `npm audit --omit=dev` | 0 Schwachstellen |
| Docker-Build für `linux/arm64` | Erfolgreich |
| Nginx-Konfigurationstest | Erfolgreich |

Der Produktionsbuild wurde am 12.11.2026 erfolgreich erstellt.

### 2.3 Sicherheitsprüfung

Die Sicherheitsprüfung der Produktionsabhängigkeiten ergab:

```text
found 0 vulnerabilities
```

Bei der vollständigen Prüfung der Abhängigkeiten wurden weiterhin drei Schwachstellen in Entwicklungs- und Testabhängigkeiten gemeldet:

- 1 × Moderate
- 2 × Critical

Diese betreffen den Entwicklungs- und Testwerkzeugbereich, insbesondere Abhängigkeiten aus dem Vitest-Umfeld.

Die Bearbeitung erfolgt separat und ist nicht Bestandteil von BUG-003.

---

## 3. Git-Release

Die Versionsanhebung wurde auf dem Branch `master` festgeschrieben.

**Release-Commit:**

```text
e1991a3 chore(release): frontend 1.2.3
```

**Git-Tag:**

```text
v1.2.3
```

Der Release-Tag wurde als *annotated Tag* erstellt.

Commit und Release-Tag wurden erfolgreich nach GitHub übertragen.

Der lokale Branch `master` war anschließend mit `origin/master` synchronisiert. Das Arbeitsverzeichnis war sauber.

Damit ist der veröffentlichte Quellcodestand eindeutig über den Git-Tag `v1.2.3` identifizierbar.

---

## 4. Erstellung des Docker-Images

### 4.1 Build

Das Docker-Image wurde auf dem Windows-Entwicklungsrechner mit Docker Desktop erstellt.

**Zielarchitektur:** `linux/arm64`  
**Nginx-Konfiguration:** `docker/nginx-prod.conf`

Build-Befehl:

```powershell
docker buildx build `
  --platform linux/arm64 `
  --build-arg NGINX_CONF=nginx-prod.conf `
  -t emc-mitglieder-frontend:1.2.3 `
  -t emc-mitglieder-frontend:prod `
  --load .
```

Der Build wurde ohne Fehler abgeschlossen.

### 4.2 Nginx-Konfigurationstest

Die Nginx-Konfiguration wurde vor dem Deployment geprüft.

```powershell
docker run --rm --platform linux/arm64 `
  --add-host emc-mitglieder-backend-prod:127.0.0.1 `
  emc-mitglieder-frontend:1.2.3 nginx -t
```

Ergebnis:

```text
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Die zusätzliche Hostzuordnung war ausschließlich für den lokalen Konfigurationstest erforderlich.

### 4.3 Export

Das Docker-Image wurde als TAR-Datei exportiert.

**Datei:** `emc-mitglieder-frontend-1.2.3.tar`  
**Lokales Archivverzeichnis:** `docker-image-archive`  
**Dateigröße:** 26.430.976 Bytes

Das Archiv wurde anschließend per SCP auf das Synology-NAS übertragen.

Die Dateigröße wurde auf dem NAS kontrolliert und stimmte mit der lokalen Datei überein.

---

## 5. Deployment auf dem NAS

### 5.1 Ausgangssituation

Vor dem Deployment lief in PROD die Frontend-Version `1.2.1`.

Der feste Docker-Tag lautete:

`emc-mitglieder-frontend:prod`

Die bisherige Image-ID war:

`ce715d6cab62`

### 5.2 Rollback-Sicherung

Vor der Umstellung wurde das bisherige PROD-Image unter folgendem Tag gesichert:

`emc-mitglieder-frontend:rollback-prod-1.2.1`

Die Sicherung wurde anhand der Docker-Image-Liste kontrolliert.

Folgende Tags zeigten auf dieselbe Image-ID:

| Docker-Tag | Image-ID |
|---|---|
| `1.2.1` | `ce715d6cab62` |
| `prod` (vor Umstellung) | `ce715d6cab62` |
| `rollback-prod-1.2.1` | `ce715d6cab62` |

Damit blieb das bisherige PROD-Image für einen möglichen Rückbau erhalten.

### 5.3 Import des neuen Images

Das neue Docker-Image wurde auf dem NAS importiert.

```bash
docker load -i ~/emc-mitglieder-frontend-1.2.3.tar
```

Der erfolgreiche Import wurde über die Docker-Image-Liste bestätigt.

**Neues Image:** `emc-mitglieder-frontend:1.2.3`  
**Image-ID:** `93a1d860a51b`

Anschließend wurde der feste PROD-Tag auf das neue Image gesetzt:

```bash
docker tag emc-mitglieder-frontend:1.2.3 emc-mitglieder-frontend:prod
```

Die Image-Zuordnung wurde kontrolliert.

### 5.4 Neuerstellung des PROD-Containers

Der Container wurde am 12.11.2026 über Portainer mit der Funktion **Recreate** neu erstellt.

**Container:** `emc-mitglieder-frontend-prod`  
**Docker-Image:** `emc-mitglieder-frontend:prod`  
**Portzuordnung:** `9082:80`  
**Neuerstellungszeitpunkt:** 12.11.2026, 14:36:30 Uhr

Ein erneuter Download des Images war nicht erforderlich, da es bereits lokal auf dem NAS vorhanden war.

Die DEV-Umgebung und das Backend wurden nicht neu bereitgestellt.

---

## 6. Technische Prüfung nach dem Deployment

### 6.1 Containerstatus und Image-ID

Portainer zeigte den PROD-Frontend-Container nach dem Recreate als `running` an.

Die tatsächlich verwendete Image-ID wurde über Docker kontrolliert:

```bash
docker inspect emc-mitglieder-frontend-prod --format '{{.Image}}'
```

Ergebnis:

```text
sha256:93a1d860a51b9a20d8e289ce2ad84d6d637a36207077a305286ef6bb073a5183
```

Diese Image-ID stimmt mit dem importierten Release-Image `1.2.3` überein.

Damit wurde bestätigt, dass der laufende PROD-Container tatsächlich die neue Version verwendet.

### 6.2 Systeminformationen

Die Systeminformationen der PROD-Anwendung zeigten:

| Eigenschaft | Wert |
|---|---|
| Umgebung | PROD |
| Frontend-Version | `1.2.3` |
| Frontend-Build | 12.11.2026, 14:24:03 |
| Backend-Version | `1.2.1` |
| Spring-Profil | `prod` |
| Backend-Verbindung | Erfolgreich |

Die angezeigten Versions- und Umgebungsinformationen waren korrekt.

---

## 7. Funktionstests

Nach dem Deployment wurde die Anwendung im Browser geprüft.

| Prüfung | Ergebnis |
|---|---|
| Aufruf der PROD-Anwendung | Erfolgreich |
| Anmeldung als Admin | Erfolgreich |
| Anzeige der Systeminformationen | Korrekt |
| Backend-Verbindung | Erfolgreich |
| Mitgliederverzeichnis | Funktioniert |
| PDF-Erzeugung Mitgliederverzeichnis | Erfolgreich |
| Geburtstagsliste | Funktioniert |
| PDF-Erzeugung Geburtstagsliste | Erfolgreich |
| Weißer Bildschirm | Nicht aufgetreten |

Bei den durchgeführten Prüfungen wurden keine Funktionsstörungen festgestellt.

---

## 8. Prüfung der Cache-Control-Korrektur

### 8.1 Prüfziel

Im Rahmen von BUG-003 wurde die Nginx-Konfiguration so geändert, dass die HTML-Einstiegsdatei `index.html` mit folgendem HTTP-Header ausgeliefert wird:

```http
Cache-Control: no-cache
```

Damit muss ein Browser eine zwischengespeicherte HTML-Einstiegsdatei vor erneuter Verwendung beim Server validieren.

### 8.2 Prüfung in PROD

Die HTTP-Antwort wurde direkt auf dem NAS geprüft.

**Prüfbefehl:**

```bash
curl -I http://localhost:9082/index.html
```

**Ergebnis vom 12.11.2026:**

```http
HTTP/1.1 200 OK
Server: nginx/1.31.8
Date: Thu, 12 Nov 2026 13:41:32 GMT
Content-Type: text/html
Content-Length: 469
Last-Modified: Thu, 12 Nov 2026 13:24:05 GMT
Connection: keep-alive
ETag: "6af5bdf5-1d5"
Cache-Control: no-cache
Accept-Ranges: bytes
```

Die Prüfung bestätigt, dass die Cache-Control-Korrektur im laufenden PROD-Container aktiv ist.

### 8.3 Bewertung zu BUG-003

Die Cache-Control-Korrektur wurde erfolgreich in DEV und PROD bereitgestellt.

Die DEV-Beobachtung verlief ohne erneute Störung. Auch die Funktionstests nach dem PROD-Deployment waren erfolgreich.

Die fehlende Cache-Steuerung wurde als technischer Mangel behoben.

Ob dieser Mangel die alleinige Ursache sämtlicher zuvor beobachteter weißer Browserseiten war, konnte nicht abschließend nachgewiesen werden.

Auf Grundlage der erfolgreichen Umsetzung und Prüfungen wurde BUG-003 am 12.11.2026 auf **Erledigt** gesetzt.

---

## 9. Auswirkungen und Rückbaumöglichkeit

### 9.1 Auswirkungen

Das Deployment betraf ausschließlich das Frontend.

Keine Änderungen erfolgten an:

- Java-/Spring-Boot-Backend.
- Datenbankschema und Datenbestand.

Die bestehenden Backend-Verbindungen und die Docker-Netzwerkkonfiguration blieben unverändert.

### 9.2 Rollback

Für einen möglichen Rückbau steht das bisherige PROD-Image weiterhin zur Verfügung:

`emc-mitglieder-frontend:rollback-prod-1.2.1`

**Image-ID:** `ce715d6cab62`

Ein Rückbau würde die erneute Zuordnung des festen Docker-Tags `:prod` zum gesicherten Image und anschließend ein Recreate des PROD-Frontend-Containers über Portainer erfordern.

Ein Rollback war beim Deployment vom 12.11.2026 nicht erforderlich.

---

## 10. Dokumentation und Nacharbeiten

### 10.1 BUG-003

Die technische Umsetzung und der Abschluss des Fehlers sind dokumentiert unter:

`BUG-003-frontend-browser-cache.md`

**Status:** Erledigt  
**Abschlussdatum:** 12.11.2026

### 10.2 DEV-Deployment

Die vorherige Bereitstellung auf DEV ist dokumentiert unter:

`frontend-1.2.3-SNAPSHOT-dev-deployment-ergebnis.md`

### 10.3 Release-Dokumentation

Die Frontend-Version `1.2.3` ist im `CHANGELOG.md` und gegebenenfalls in der übergreifenden Release-Dokumentation nachzuführen.

Die Version `1.2.2` wurde als eigenständiges PROD-Release übersprungen.

### 10.4 Offene Sicherheitsaufgabe

Drei bekannte Schwachstellen in Entwicklungs- und Testabhängigkeiten bestehen weiterhin.

Die Bearbeitung erfolgt separat, insbesondere im Zusammenhang mit einer möglichen Aktualisierung der Vitest-Hauptversion.

Diese Aufgabe ist nicht Bestandteil des abgeschlossenen BUG-003.

---

## 11. Abschlussbewertung

Das PROD-Deployment der Frontend-Version `1.2.3` wurde am **12.11.2026 erfolgreich durchgeführt**.

Die wesentlichen Ergebnisse:

- Der Git-Release ist mit `v1.2.3` eindeutig gekennzeichnet.
- Das ARM64-Docker-Image wurde erfolgreich erstellt und auf dem NAS bereitgestellt.
- Der PROD-Container verwendet nachweislich das neue Image.
- Die Funktionstests einschließlich der PDF-Berichte waren erfolgreich.
- Der HTTP-Header `Cache-Control: no-cache` wurde in PROD bestätigt.
- Das bisherige PROD-Image `1.2.1` steht für einen möglichen Rollback bereit.

**Gesamtergebnis:** PROD-Deployment erfolgreich abgeschlossen und abgenommen.

**Release-Version:** `1.2.3`  
**Deployment-Datum:** 12.11.2026  
**Status:** Abgeschlossen
