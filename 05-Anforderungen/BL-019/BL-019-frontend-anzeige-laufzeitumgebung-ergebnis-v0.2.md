# BL-019 – Frontend-Anzeige der Laufzeitumgebung – Ergebnisdokument

**Anforderungs-ID:** BL-019  
**Dokumentstand:** 04.11.2026  
**Status:** Abgeschlossen – produktiv mit Release `1.2.1`; Einschränkung der Kopierfunktion an BL-026 übergeben  
**Referenz:** `BL-019-frontend-anzeige-laufzeitumgebung_v0.3.md` (fachlich freigegeben)

## 1. Zusammenfassung

Die Frontend-Umsetzung von BL-019 wurde zunächst lokal abgeschlossen und anschließend mit Release `1.2.1` in DEV und PROD bereitgestellt. Die Kopfzeile zeigt die vom Backend gemeldete Laufzeitumgebung an; ein Info-Dialog stellt angemeldeten Benutzern System- und Versionsinformationen bereit und ermöglicht deren Übernahme in die Zwischenablage. Die bisherigen statischen `localhost`- und Proxy-Angaben wurden aus der Kopfzeile entfernt.

Die ursprüngliche Implementierung wurde lokal gebaut und geprüft und auf einem Frontend-Feature-Branch committed. Die Funktion wurde später in den Release-Stand `1.2.1` integriert und am 04.11.2026 in PROD bereitgestellt. Die bekannte Einschränkung der Zwischenablagefunktion im HTTP-Betrieb ist im Anforderungsdokument BL-026 als gesonderter HTTPS-Abnahmeprüfpunkt aufgenommen.

## 2. Umgesetzte Funktionen

### 2.1 Kopfzeile und Laufzeitumgebung

- Überarbeitete Kopfzeile mit Bezeichnung **EMC Mitgliederverwaltung**, vorläufigem Symbol, rollenabhängiger Navigation, Benutzername, Rolle, Umgebungskennzeichnung, **Info** und **Abmelden**.
- Umgebung wird aus `environment` der authentifizierten Backend-Schnittstelle `/api/system/info` bezogen; zulässig sind `LOCAL`, `DEV` und `PROD`.
- Unterschiedliche farbliche Kennzeichnung der Umgebungen; Bedienelemente in der Kopfzeile auf einheitlich etwa 36 px Höhe ausgerichtet.
- Keine Ableitung der Umgebung aus URL, Frontend-Build oder Datenbankverbindung.
- Bisherige statische Angaben `localhost` und `Proxy → localhost:8080` sowie die dauerhafte Versionsanzeige wurden aus der Kopfzeile entfernt.
- Bei fehlender gültiger Umgebungsinformation wird **Unbekannt** angezeigt; ein fehlgeschlagener Folgeabruf bei zuvor erfolgreichem Abruf wird an der Kennzeichnung mit `*` kenntlich gemacht.

### 2.2 Info-Dialog

- Für angemeldete Benutzer unabhängig von der Rolle verfügbar.
- Anzeige von Frontend-Version und Frontend-Build-Zeitpunkt sowie Backend-Version, Backend-Build-Zeitpunkt, Laufzeitumgebung, aktiven Spring-Profilen, Benutzername und Rolle.
- Deutschsprachige Beschriftungen und Statusmeldungen.
- Backend-Informationen werden beim Laden des Anwendungslayouts abgefragt und beim Öffnen des Info-Dialogs erneut aktualisiert; keine zyklischen Hintergrundabfragen.
- Initialer Ladezustand, erfolgreicher Abruf, laufende Aktualisierung und Fehlerzustände werden unterschieden.
- Bei fehlgeschlagener Aktualisierung bleiben zuletzt erfolgreich geladene Backend-Daten verfügbar und werden als nicht erneut bestätigt gekennzeichnet.
- Funktion **Informationen kopieren** erstellt einen lesbaren Diagnosetext mit Systeminformationen, Abrufstatus und Zeitangaben einschließlich Erfassungszeitpunkt.
- Dialog kann geschlossen werden; auch Escape-Taste und Klick außerhalb des Dialoginhalts wurden berücksichtigt.

### 2.3 Technische Anbindung

- Gemeinsame Bereitstellung der Backend-Systeminformationen für Kopfzeile und Dialog über einen React-Hook.
- Abruf über `/api/system/info` unter Verwendung des bestehenden Authentifizierungskontexts (`credentials: include`).
- Prüfung des zurückgelieferten Umgebungswerts gegen die zulässigen Kennungen.
- Abbruchmöglichkeit laufender Abrufe beim Verlassen der Komponente; Berücksichtigung des React-StrictMode-Verhaltens.
- Frontend-Version aus `package.json`; Frontend-Build-Zeitpunkt über die Vite-Konfiguration bereitgestellt. Im lokalen Entwicklungsbetrieb entspricht dieser Zeitstempel dem Start-/Konfigurationszeitpunkt von Vite, nicht einem produktiven Auslieferungsbuild.
- Benutzername und Rolle stammen aus dem vorhandenen Anmeldekontext.

## 3. Geänderte Frontend-Dateien

| Datei | Änderung |
|---|---|
| `package.json` | Im ursprünglichen Feature-Stand Frontend-Version auf `1.2.1-SNAPSHOT` gesetzt; produktiv mit `1.2.1` ausgeliefert |
| `package-lock.json` | Versionsangabe synchronisiert |
| `vite.config.js` | Frontend-Build-Zeitpunkt bereitgestellt |
| `src/hooks/useSystemInfo.js` | Neuer Hook für Abruf, Aktualisierung und Fehlerbehandlung |
| `src/components/common/EnvironmentBadge.jsx` | Umgebungskennzeichnung statt statischer Verbindungsinformationen |
| `src/components/common/SystemInfoDialog.jsx` | Neuer Dialog für Systeminformationen und Kopierfunktion |
| `src/components/layouts/AppLayout.jsx` | Überarbeitete Kopfzeile und Integration von Hook und Dialog |

Am Backend wurde für BL-019 keine Codeänderung committed. Die bestehende Backend-Schnittstelle wurde genutzt; für die lokale Prüfung wurde das Spring-Profil `local` verwendet. Die lokale Backend-Instanz kann dabei weiterhin auf die DEV-Datenbank zugreifen.

## 4. Qualitätssicherung und Prüfergebnisse

### 4.1 Automatisierte Prüfungen

| Prüfung | Ergebnis |
|---|---|
| `npm run build` | Erfolgreich, Frontend `1.2.1-SNAPSHOT` |
| `npm run test -- --run` | Im ursprünglichen Feature-Stand erfolgreich: 64 Tests in 7 Testdateien; im Release-Kontext `1.2.1` insgesamt 65 erfolgreiche Frontend-Tests |
| `git diff --check` | Ohne Beanstandung |

Die vorhandenen Tests decken insbesondere Hilfs- und Validierungsfunktionen ab. **Neue automatisierte Hook- oder Komponententests für BL-019 wurden nicht eingeführt.** Die dafür erforderliche Testinfrastruktur und die entsprechenden Tests sind dem separaten Thema **BL-021 – Frontend-Komponententests** zugeordnet.

### 4.2 Manuelle LOCAL-Prüfungen

- Darstellung und Bedienung der neuen Kopfzeile im Browser geprüft.
- Rollen **ADMIN** und **EDITOR** geprüft: bestehende rollenabhängige Navigation blieb erhalten; der Info-Dialog war für beide Rollen zugänglich.
- Anzeige der Systeminformationen und Übernahme in die Zwischenablage geprüft; kopierter Text enthielt die vorgesehenen Informationen und Zeitangaben.
- Beim Öffnen des Dialogs wurde im Browser-Netzwerkprotokoll genau **ein** GET auf `/api/system/info` mit HTTP **200** beobachtet.
- Backend-Ausfall während einer bereits angemeldeten Sitzung geprüft: Bei erneuter Aktualisierung wurden zuletzt bekannte Informationen weiterhin angezeigt und der Fehlerzustand kenntlich gemacht.
- Backend nach dem Ausfalltest wieder gestartet; anschließende Funktionsprüfung erfolgreich.

**Prüfgrenze der ursprünglichen LOCAL-Tests:** Diese Tests allein belegen die Anzeige in DEV und PROD nicht. Die Anwendung wurde anschließend in beiden Umgebungen bereitgestellt; eine vollständige, einzeln protokollierte Prüfliste zu BL-019 für DEV und PROD liegt diesem Ergebnisdokument nicht zugrunde. Die erfolgreiche produktive Bereitstellung ist daher von den oben konkret belegten LOCAL-Prüfungen zu unterscheiden.

## 5. Bekannte Einschränkungen und Abgrenzungen

- **Zwischenablagefunktion unter HTTP:** Die Schaltfläche **„Informationen kopieren“** ist implementiert und wurde lokal erfolgreich geprüft. Im bisherigen HTTP-Betrieb von DEV und PROD kann der Zugriff auf die Browser-Zwischenablage aufgrund des erforderlichen sicheren Browser-Kontexts scheitern. Die Prüfung der Funktion nach der HTTPS-Umstellung in beiden Umgebungen ist ausdrücklich in **BL-026 – HTTPS-Einführung** als Abnahmekriterium aufgenommen. BL-019 wird mit dieser dokumentierten Einschränkung abgeschlossen.

- Wenn das Backend bereits beim Neuladen der Anwendung nicht erreichbar ist, kann die bestehende Anmeldung scheitern. Der Info-Dialog ist dann nicht erreichbar. Das betrifft das vorhandene Login-/Fehlerverhalten und ist kein Bestandteil von BL-019.
- Eine permanente Überwachung der Backend-Erreichbarkeit wurde bewusst nicht eingeführt. Der Status wird beim Layout-Laden und beim Öffnen des Info-Dialogs aktualisiert.
- Die Symbole für Verein und Benutzer sind vorläufig; eine spätere Ersetzung durch EMC-Logo beziehungsweise neutrales SVG-Benutzersymbol ist vorgesehen, aber nicht Bestandteil der abgeschlossenen LOCAL-Implementierung.
- Automatisierte React-Hook- und Komponententests sind BL-021 vorbehalten.
- Die gesondert diskutierte Vereinheitlichung fachlicher Datumswerte im Backend ist **nicht** Bestandteil von BL-019. Die gemeinsame Bereitstellung mit BL-027 in Release `1.2.1` ändert diese fachliche Trennung nicht.

## 6. Git- und Versionsstand

| Merkmal | Stand |
|---|---|
| Repository | `mitglieder-frontend` |
| Feature-Branch | `feature/BL-019-frontend-laufzeitumgebung` |
| Commit | `3e9fd76` |
| Commit-Nachricht | `feat(BL-019): Laufzeitumgebung und Systeminformationen im Frontend anzeigen` |
| Arbeitsverzeichnis nach Commit | Sauber |
| Frontend-Version | Ursprünglicher Feature-Stand `1.2.1-SNAPSHOT`; produktiver Release-Stand `1.2.1` |
| Merge nach `master` | Erfolgt; Funktion im Release `1.2.1` enthalten |
| DEV-Deployment | Erfolgt vor PROD-Bereitstellung |
| PROD-Deployment | Erfolgt am 04.11.2026 mit Release `1.2.1` |
| Release-Commit (Frontend) | `6faaa61` (Release-Stand; nicht ausschließlich BL-019) |

## 7. Abschluss und verbleibende Folgearbeiten

Die im ursprünglichen Zwischenstand noch offenen Schritte (Merge, DEV-Bereitstellung, PROD-Bereitstellung) sind mit dem Release `1.2.1` erledigt. Die ursprüngliche Trennung zwischen BL-019 (Laufzeitumgebung und Info-Dialog) und BL-027 (Datumsverarbeitung) bleibt auch bei gemeinsamer Auslieferung bestehen.

Folgende Arbeiten sind bewusst außerhalb von BL-019 eingeordnet:

- **BL-026 – HTTPS-Einführung:** Prüfung der Zwischenablagefunktion **„Informationen kopieren“** in DEV und PROD nach HTTPS-Umstellung; bei weiterhin bestehendem Fehler gesonderte Fehlerbehebung.
- **BL-021 – Frontend-Komponententests:** Gegebenenfalls spätere automatisierte Tests für React-Hook und Dialog.
- **Projektsteuerung:** Status von BL-019 im Backlog nach Übernahme dieses Abschlussdokuments auf „Abgeschlossen“ setzen.

## 8. Abschlussbewertung

BL-019 wird mit Stand **04.11.2026** als abgeschlossen bewertet. Die überarbeitete Kopfzeile und der Info-Dialog wurden implementiert und mit Frontend-Release `1.2.1` produktiv bereitgestellt. Die zuvor irreführenden `localhost`- und Proxy-Angaben wurden aus der sichtbaren Kopfzeile entfernt.

Die Funktion **„Informationen kopieren“** ist Bestandteil der Umsetzung, im derzeitigen HTTP-Betrieb jedoch nicht in allen Browser-Kontexten zuverlässig nutzbar. Diese Einschränkung ist ausdrücklich dokumentiert; die Funktionsprüfung unter HTTPS wurde verbindlich in BL-026 übernommen. Daraus folgt keine rückwirkende Behauptung, die Kopierfunktion sei bereits in PROD erfolgreich getestet worden.

**Dokumentationsgrenze:** Die in Abschnitt 4.2 aufgeführten konkreten Einzeltests stammen aus der LOCAL-Prüfung vom 27.10.2026. Zusätzliche Einzeltestergebnisse für BL-019 in DEV und PROD werden hier nicht behauptet, soweit sie nicht gesondert protokolliert vorliegen.
