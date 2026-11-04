# BL-019 – Frontend-Anzeige der Laufzeitumgebung – Ergebnisdokument

**Anforderungs-ID:** BL-019  
**Dokumentstand:** 27.10.2026  
**Status:** Zwischenstand – LOCAL umgesetzt und geprüft; DEV-Abnahme ausstehend  
**Referenz:** `BL-019-frontend-anzeige-laufzeitumgebung_v0.3.md` (fachlich freigegeben)

## 1. Zusammenfassung

Die Frontend-Umsetzung von BL-019 ist lokal abgeschlossen. Die Kopfzeile zeigt die vom Backend gemeldete Laufzeitumgebung an; ein Info-Dialog stellt angemeldeten Benutzern System- und Versionsinformationen bereit und ermöglicht deren Übernahme in die Zwischenablage. Die bisherigen statischen `localhost`- und Proxy-Angaben wurden aus der Kopfzeile entfernt.

Die Implementierung wurde lokal gebaut und geprüft und auf einem Frontend-Feature-Branch committed. Ein Merge nach `master`, ein DEV-Deployment und eine DEV-Abnahme stehen noch aus. Eine PROD-Freigabe liegt nicht vor.

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
| `package.json` | Frontend-Version auf `1.2.1-SNAPSHOT` gesetzt |
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
| `npm run test -- --run` | Erfolgreich: 64 Tests in 7 Testdateien |
| `git diff --check` | Ohne Beanstandung |

Die vorhandenen Tests decken insbesondere Hilfs- und Validierungsfunktionen ab. **Neue automatisierte Hook- oder Komponententests für BL-019 wurden nicht eingeführt.** Die dafür erforderliche Testinfrastruktur und die entsprechenden Tests sind dem separaten Thema **BL-021 – Frontend-Komponententests** zugeordnet.

### 4.2 Manuelle LOCAL-Prüfungen

- Darstellung und Bedienung der neuen Kopfzeile im Browser geprüft.
- Rollen **ADMIN** und **EDITOR** geprüft: bestehende rollenabhängige Navigation blieb erhalten; der Info-Dialog war für beide Rollen zugänglich.
- Anzeige der Systeminformationen und Übernahme in die Zwischenablage geprüft; kopierter Text enthielt die vorgesehenen Informationen und Zeitangaben.
- Beim Öffnen des Dialogs wurde im Browser-Netzwerkprotokoll genau **ein** GET auf `/api/system/info` mit HTTP **200** beobachtet.
- Backend-Ausfall während einer bereits angemeldeten Sitzung geprüft: Bei erneuter Aktualisierung wurden zuletzt bekannte Informationen weiterhin angezeigt und der Fehlerzustand kenntlich gemacht.
- Backend nach dem Ausfalltest wieder gestartet; anschließende Funktionsprüfung erfolgreich.

**Prüfgrenze:** Die korrekte Anzeige von `DEV` und `PROD` im jeweiligen Zielbetrieb ist durch diese LOCAL-Prüfungen noch nicht nachgewiesen. Insbesondere die DEV-Abnahme bleibt offen.

## 5. Bekannte Einschränkungen und Abgrenzungen

- Wenn das Backend bereits beim Neuladen der Anwendung nicht erreichbar ist, kann die bestehende Anmeldung scheitern. Der Info-Dialog ist dann nicht erreichbar. Das betrifft das vorhandene Login-/Fehlerverhalten und ist kein Bestandteil von BL-019.
- Eine permanente Überwachung der Backend-Erreichbarkeit wurde bewusst nicht eingeführt. Der Status wird beim Layout-Laden und beim Öffnen des Info-Dialogs aktualisiert.
- Die Symbole für Verein und Benutzer sind vorläufig; eine spätere Ersetzung durch EMC-Logo beziehungsweise neutrales SVG-Benutzersymbol ist vorgesehen, aber nicht Bestandteil der abgeschlossenen LOCAL-Implementierung.
- Automatisierte React-Hook- und Komponententests sind BL-021 vorbehalten.
- Die gesondert diskutierte Vereinheitlichung fachlicher Datumswerte im Backend ist **nicht** Bestandteil von BL-019. Eine mögliche gemeinsame Bereitstellung mit BL-019 unter `1.2.1-SNAPSHOT` ändert diese fachliche Trennung nicht.

## 6. Git- und Versionsstand

| Merkmal | Stand |
|---|---|
| Repository | `mitglieder-frontend` |
| Feature-Branch | `feature/BL-019-frontend-laufzeitumgebung` |
| Commit | `3e9fd76` |
| Commit-Nachricht | `feat(BL-019): Laufzeitumgebung und Systeminformationen im Frontend anzeigen` |
| Arbeitsverzeichnis nach Commit | Sauber |
| Frontend-Version | `1.2.1-SNAPSHOT` |
| Merge nach `master` | Ausstehend |
| DEV-Deployment | Ausstehend |
| PROD-Deployment | Nicht freigegeben |

## 7. Offene Schritte bis zum Abschluss

1. Separate Datumsanforderung fachlich und technisch eingrenzen; danach entscheiden, ob beide Anforderungen gemeinsam in `1.2.1-SNAPSHOT` nach DEV gelangen.
2. BL-019-Feature-Branch nach abgestimmtem Vorgehen in `master` integrieren.
3. DEV-Deployment der vorgesehenen Frontend-Version durchführen.
4. In DEV insbesondere Umgebungskennzeichnung, Info-Dialog, Kopierfunktion, Benutzerrollen und unveränderte Navigation prüfen.
5. DEV-Prüfergebnisse und tatsächlich ausgelieferte Versionsstände in diesem Ergebnisdokument ergänzen.
6. Backlog und gegebenenfalls Deployment-Dokumentation aktualisieren; `CHANGELOG.md` zum passenden Releasezeitpunkt fortschreiben.
7. PROD-Deployment nur nach gesonderter Entscheidung.

## 8. Abschlussbewertung (vorläufig)

Die LOCAL-Implementierung erfüllt nach den bisherigen Prüfungen die wesentlichen Anforderungen von BL-019. Die Anforderung ist **noch nicht vollständig abgenommen**, weil die Prüfung in DEV aussteht. Dieses Ergebnisdokument wird nach der DEV-Abnahme vervollständigt und erst dann als Abschlussdokument geführt.
