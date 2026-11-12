# Frontend 1.2.3-SNAPSHOT – DEV-Deployment und Prüfergebnis

**Datum:** 09.11.2026  
**Projekt:** EMC Mitgliederverwaltung  
**Komponente:** Frontend (React/Vite, Nginx)  
**Zielumgebung:** DEV  
**Status:** Erfolgreich bereitgestellt – Beobachtungsphase läuft

## 1. Anlass

Bereitstellung der Frontend-Version `1.2.3-SNAPSHOT` auf DEV.

Die Version enthält insbesondere:

- Die Korrektur der Browser-Cache-Behandlung für `index.html` im Rahmen von BUG-003.
- Die Aktualisierung der Frontend-Abhängigkeiten zur Behebung bekannter Sicherheitslücken in Produktionsabhängigkeiten.

## 2. Vorbereitung und Qualitätssicherung

Folgende Prüfungen wurden vor dem Deployment erfolgreich durchgeführt:

| Prüfung                        | Ergebnis              |
| ------------------------------ | --------------------- |
| `npm ci`                       | Erfolgreich           |
| Frontend-Unit-Tests            | 65 von 65 erfolgreich |
| `npm run build`                | Erfolgreich           |
| `npm audit --omit=dev`         | 0 Schwachstellen      |
| Docker-Build für `linux/arm64` | Erfolgreich           |
| Nginx-Konfigurationstest       | Erfolgreich           |

Die Änderungen wurden in `master` übernommen und nach GitHub übertragen.

Relevante Commits:

- `f4f0bc2` – Cache-Control-Korrektur
- `534d9c5` – Aktualisierung der Abhängigkeiten
- `48cbde3` – Versionsanhebung auf `1.2.3-SNAPSHOT`

## 3. Deployment

Das Docker-Image wurde lokal erstellt, als TAR-Datei exportiert und auf das NAS übertragen.

**Image:** `emc-mitglieder-frontend:1.2.3-SNAPSHOT`  
**Docker-Tag für DEV:** `emc-mitglieder-frontend:dev`  
**Image-ID:** `848111c2d134`

Der DEV-Container wurde über Portainer neu erstellt.

**Container:** `emc-mitglieder-frontend-dev`  
**Portzuordnung:** `8082:80`

Vor dem Deployment wurde das bisherige DEV-Image unter folgendem Tag gesichert:

`emc-mitglieder-frontend:rollback-dev-1.2.2-SNAPSHOT`

Die PROD-Umgebung wurde nicht verändert.

## 4. Prüfergebnisse nach dem Deployment

| Prüfung                         | Ergebnis                                       |
| ------------------------------- | ---------------------------------------------- |
| Containerstatus                 | Running                                        |
| Tatsächlich verwendete Image-ID | Korrekt                                        |
| Frontend-Version                | `1.2.3-SNAPSHOT`                               |
| Frontend-Build                  | 09.11.2026, 16:35:09                           |
| Backend-Version                 | `1.2.1-SNAPSHOT`                               |
| Backend-Verbindung              | Erfolgreich                                    |
| Anmeldung                       | Erfolgreich                                    |
| Mitgliederverzeichnis           | Funktioniert                                   |
| Geburtstagsliste                | Funktioniert                                   |
| Weißer Bildschirm               | Bei den bisherigen Prüfungen nicht aufgetreten |

### 4.1 Prüfung BUG-003

Die HTTP-Antwort von `http://localhost:8082/index.html` wurde geprüft.

Ergebnis:

```http
HTTP/1.1 200 OK
Cache-Control: no-cache
```

Damit ist bestätigt, dass die geänderte Nginx-Konfiguration im laufenden DEV-Container aktiv ist.

Eine dauerhafte Behebung von BUG-003 ist damit noch nicht abschließend nachgewiesen.

## 5. Offene Punkte

### 5.1 Beobachtung von BUG-003

Die DEV-Umgebung wird zunächst einige Tage im normalen Betrieb beobachtet.

BUG-003 bleibt bis zum Abschluss der Beobachtung offen.

### 5.2 Entwicklungsabhängigkeiten

Nach der Aktualisierung der Abhängigkeiten bestehen weiterhin drei gemeldete Schwachstellen im Entwicklungs- und Testwerkzeugbereich.

Betroffen sind insbesondere Abhängigkeiten aus dem Vitest-Umfeld. Eine vollständige Bereinigung erfordert voraussichtlich ein Upgrade auf eine neue Vitest-Hauptversion.

Die Bearbeitung wird separat vorgesehen.

### 5.3 PROD-Release

PROD verbleibt vorerst auf Frontend-Version `1.2.1`.

Nach erfolgreicher Beobachtungsphase soll die Freigabe der Version `1.2.3` für PROD erfolgen. Die Version `1.2.2` wird damit als eigenständiges PROD-Release übersprungen.

## 6. Abschlussbewertung

Das DEV-Deployment vom 09.11.2026 wurde technisch erfolgreich durchgeführt.

Die bisherigen Funktionsprüfungen waren erfolgreich. Die Cache-Control-Korrektur ist aktiv.

**Ergebnis:** DEV-Deployment abgeschlossen; Freigabe für PROD noch ausstehend.