# BL-019 – Frontend-Anzeige der Laufzeitumgebung

**Anforderungs-ID:** BL-019  
**Dokumentversion:** v0.4  
**Stand:** 04.11.2026  
**Status:** Abgeschlossen am 04.11.2026  
**Priorität:** Mittel  
**Roadmap-Bezug:** P1 – Technische Produktreife

## 1. Ziel

Das Frontend der EMC Mitgliederverwaltung soll die tatsächlich verwendete Backend-Laufzeitumgebung eindeutig anzeigen. Unterstützt werden **LOCAL**, **DEV** und **PROD**.

Verwechslungen zwischen Entwicklungs- und Produktivumgebung sollen vermieden werden. Zusätzlich sollen alle angemeldeten Benutzer über einen **Info-Dialog** die für eine Fehlermeldung an die Entwicklung benötigten Systeminformationen einsehen und kopieren können.

## 2. Ausgangssituation

Die EMC Mitgliederverwaltung besteht aus einem React/Vite-Frontend und einem Java/Spring-Boot-Backend. Es bestehen getrennte Backend-Ausführungsumgebungen LOCAL (lokal aus IntelliJ), DEV (Docker/NAS) und PROD (Docker/NAS).

Das Backend stellt seit BL-007 Betriebsinformationen über den authentifizierten Endpunkt `/api/system/info` bereit. Die Antwort enthält `backendVersion`, `environment`, `activeProfiles` und `buildTime`.

Die bestehende Frontend-Kopfzeile enthält eine Versionsanzeige und die fest kodierten Angaben `localhost` sowie `Proxy → localhost:8080`. Diese erscheinen auch in PROD und sind deshalb irreführend. Die bestehende Frontend-Version stammt aus der `package.json` und wird über Vite bereitgestellt; ein Frontend-Build-Zeitpunkt ist bislang nicht verfügbar.

Die Backend-Umgebung wird anhand aktiver Spring-Profile ermittelt. Für LOCAL ist das Spring-Profil `local` vorgesehen; die lokale Backend-Instanz kann dabei weiterhin die DEV-Datenbank auf dem NAS verwenden. Die Datenbankumgebung bestimmt **nicht** die Anzeige LOCAL/DEV/PROD.

Die Backend-Version stammt aus den beim Maven-Build erzeugten Build-Informationen. Die Bereitstellung der Backend-Version soll ohne zusätzliche Backend-Codeänderung über die bestehende Schnittstelle erfolgen.

## 3. Fachliche Anforderungen

### FA-019-01 – Anzeige der Laufzeitumgebung

Die aktuell verwendete **Backend-Ausführungsumgebung** muss in der Kopfzeile dauerhaft und eindeutig als **LOCAL**, **DEV** oder **PROD** angezeigt werden. Eine optisch unterscheidbare Kennzeichnung ist vorgesehen. Die Anzeige darf nicht mit der verwendeten Datenbankumgebung verwechselt werden.

### FA-019-02 – Verlässliche Informationsquelle

Maßgeblich ist das Feld `environment` der bestehenden Backend-Schnittstelle `/api/system/info`. Die Anzeige darf weder aus der Frontend-Buildkonfiguration noch aus der aufgerufenen URL oder aus der Datenbankverbindung abgeleitet werden.

### FA-019-03 – Gestaltung der Kopfzeile

Die Kopfzeile wird im abgestimmten, übersichtlichen Stil gestaltet und enthält:

- links die Bezeichnung **EMC Mitgliederverwaltung** mit vorläufigem Symbol; ein späteres echtes Vereinslogo soll ohne grundlegende Layoutänderung einsetzbar sein,
- die bestehende Navigation (insbesondere **Reporting** und **Benutzer**) entsprechend den jeweiligen Berechtigungen,
- den angemeldeten **Benutzernamen und die Rolle**,
- die farblich unterscheidbare **Laufzeitumgebung**,
- einen Aufruf **Info** und die bestehende Funktion **Abmelden**.

**Frontend- und Backend-Versionsnummern werden nicht mehr dauerhaft in der Kopfzeile angezeigt**, sondern ausschließlich im Info-Dialog.

Die Überarbeitung ist auf die Kopfzeile beschränkt; eine allgemeine Neugestaltung der Anwendung ist nicht beabsichtigt.

### FA-019-04 – Info-Dialog für alle angemeldeten Benutzer

Alle angemeldeten Benutzer, unabhängig von ihrer Rolle, können einen Info-Dialog mit Systeminformationen öffnen. Der Dialog dient insbesondere dazu, bei Problemen den verwendeten technischen Stand an die Entwicklung weiterzugeben.

Er zeigt mindestens:

| Information | Herkunft |
|---|---|
| Laufzeitumgebung (LOCAL/DEV/PROD) | Backend-API |
| Frontend-Version | Frontend-Build / `package.json` |
| Frontend-Build-Zeitpunkt | Beim Vite-Build erzeugte Information |
| Backend-Version | Backend-API |
| Backend-Build-Zeitpunkt | Backend-API |
| Aktive Spring-Profile | Backend-API |
| Benutzername | Bestehender Anmeldekontext |
| Rolle | Bestehender Anmeldekontext |

Benutzername und Rolle sollen aus dem bestehenden Authentifizierungskontext stammen; eine zusätzliche Backend-Abfrage ist dafür nicht vorgesehen.

### FA-019-05 – Systeminformationen kopieren

Der Info-Dialog bietet eine Funktion **„Informationen kopieren“**. Sie übernimmt die angezeigten Systeminformationen in lesbarer, deutsch beschrifteter Textform in die Zwischenablage, damit sie beispielsweise in einer Fehlermeldung weitergegeben werden können.

Ein Freitextfeld, eine automatische Fehlermeldungsübermittlung und ein automatischer Logexport sind **nicht** Bestandteil von BL-019.

### FA-019-06 – Fehlerverhalten

Kann die Backend-Laufzeitumgebung nicht ermittelt werden, darf keine unbelegte oder möglicherweise falsche Umgebung angezeigt werden. Stattdessen wird die Umgebung als **„Unbekannt“** kenntlich gemacht. Nicht verfügbare Backend-Informationen werden im Info-Dialog verständlich gekennzeichnet; die verfügbaren Frontend- und Benutzerinformationen bleiben zugänglich.

Unbekannte oder ungültige API-Umgebungswerte dürfen nicht ungeprüft als LOCAL, DEV oder PROD interpretiert werden. Ein fehlgeschlagener Abruf der Systeminformationen darf die übrige Bedienbarkeit einer bereits geladenen Oberfläche nicht zusätzlich beeinträchtigen. Beim erstmaligen Abruf wird „Unbekannt“ angezeigt; zuvor erfolgreich geladene Backend-Informationen bleiben bei einem späteren Fehler mit eindeutiger Kennzeichnung als zuletzt bekannte Werte erhalten. Beim Öffnen des Info-Dialogs erfolgt ein erneuter Abruf, jedoch keine dauerhafte Hintergrundüberwachung. Ist das Backend bei einem vollständigen Neuladen nicht erreichbar, kann die bestehende Authentifizierungsprüfung zur Anmeldeseite führen; dies ist keine durch BL-019 zu ändernde Funktion.

### FA-019-07 – Deutschsprachige Benutzeroberfläche

Neue Beschriftungen und Meldungen sind deutschsprachig. Technische Kennungen wie LOCAL, DEV, PROD sowie Versions- und Profilwerte bleiben unverändert.

### FA-019-08 – Rückbau der bisherigen technischen Verbindungsanzeige

Die fest kodierten Angaben `localhost`, `Proxy → localhost:8080` und vergleichbare interne Verbindungsdetails entfallen aus der sichtbaren Kopfzeile. Sie werden nicht als Ersatz für die echte Umgebungsanzeige verwendet und nicht unverändert in den Info-Dialog verschoben.

Der Rückbau betrifft ausschließlich die Darstellung, nicht die technische Frontend-Backend-Kommunikation.

## 4. Technische Anforderungen

### TA-019-01 – Nutzung bestehender Schnittstellen

Der vorhandene authentifizierte Endpunkt `/api/system/info` wird für Backend-Version, Backend-Build-Zeitpunkt, aktive Profile und Laufzeitumgebung verwendet. Ein zusätzlicher Backend-Endpunkt ist nach bisherigem Stand nicht erforderlich.

### TA-019-02 – Wiederverwendung vorhandener Funktionen

Die vorhandene Anbindung für Systeminformationen und die bestehende Frontend-Versionsbereitstellung sollen wiederverwendet werden. Die Benutzer- und Rolleninformationen sollen aus dem bestehenden Anmeldekontext bezogen werden, sofern dieser sie bereitstellt. Zusätzliche Strukturen sind nur bei nachgewiesenem Bedarf einzuführen.

### TA-019-03 – Frontend-Build-Zeitpunkt

Beim Vite-Build wird ein eindeutiger Build-Zeitpunkt erzeugt und der Anwendung zur Anzeige im Info-Dialog bereitgestellt. Er muss den **tatsächlich ausgelieferten Frontend-Build** beschreiben und darf nicht bei jedem Seitenaufruf neu entstehen. Im lokalen Vite-Entwicklungsbetrieb entspricht der Wert dem Zeitpunkt, zu dem die Vite-Konfiguration geladen wurde. Die bestehende Frontend-Versionsnummer bleibt erhalten.

### TA-019-04 – Keine Beeinträchtigung bestehender Funktionen

Navigation, Berechtigungen, Anmeldung/Abmeldung, vorhandene Fachfunktionen und Backend-Kommunikation dürfen nicht beeinträchtigt werden. Unnötige Refactorings sind ausgeschlossen.

### TA-019-05 – Robustes Fehlerverhalten und Datenschutz

Der Info-Dialog und die Kopfzeile bleiben bei fehlgeschlagenem Abruf von `/api/system/info` nutzbar. Es werden keine Passwörter, Tokens, Session-Cookies, vollständigen Datenbank-Verbindungszeichenfolgen oder sonstigen Geheimnisse angezeigt oder kopiert.

### TA-019-06 – LOCAL-Konfiguration

Die lokale Startkonfiguration soll das Spring-Profil `local` verwenden, während die Datenbankverbindung über die bestehenden Umgebungsvariablen weiterhin auf die DEV-Datenbank auf dem NAS zeigen kann. Für die Umsetzung ist keine zusätzliche lokale Datenbank vorgesehen.

### TA-019-07 – Gemeinsamer Abruf und Aktualisierung

Die Backend-Systeminformationen sollen beim Laden des Anwendungslayouts einmal abgerufen und der Kopfzeile sowie dem Info-Dialog gemeinsam bereitgestellt werden. Beim Öffnen des Dialogs wird einmal erneut abgefragt. Ein fehlgeschlagener erneuter Abruf erhält die letzte erfolgreiche Antwort und kennzeichnet sie als nicht aktuell bestätigt. Ein Abruf kann beim Verlassen der Komponente abgebrochen werden.

### TA-019-08 – Kopierbare Diagnoseinformationen

Der Kopiertext enthält neben den sichtbaren System- und Benutzerinformationen den Informationsstand, den Zeitpunkt des letzten erfolgreichen Backend-Abrufs und den Zeitpunkt der Erfassung. Geheimnisse und interne Verbindungszeichenfolgen werden nicht übernommen.

## 5. Technische Festlegungen

Für die Umsetzung gelten folgende Festlegungen:

- **Benutzerinformationen:** Benutzername und Rolle werden aus dem vorhandenen Anmeldekontext übernommen; hierfür ist keine zusätzliche Schnittstelle vorgesehen.
- **Kopfzeile:** Die bestehende rollenabhängige Navigation bleibt erhalten. Bedienelemente und Kennzeichnungen sollen einheitlich hoch und vertikal ausgerichtet sein (Richtwert: 36 px).
- **Symbole:** Ein vorläufiges Vereinssymbol ist zulässig. Das spätere EMC-Logo und ein neutrales Benutzersymbol sollen ohne grundlegende Layoutänderung als Bilddatei oder SVG eingebunden werden können.
- **Systeminformationen:** Datenbeschaffung und Darstellung sind zu trennen; Kopfzeile und Info-Dialog verwenden denselben Informationsstand.
- **Zeitangaben:** Datums- und Uhrzeitwerte werden im Info-Dialog deutsch formatiert. Im lokalen Vite-Entwicklungsbetrieb ist der Frontend-Zeitstempel als Start-/Konfigurationszeitpunkt zu verstehen.
- **Fehlerbehandlung:** Der initiale Abruf und die Aktualisierung beim Öffnen des Info-Dialogs müssen unterschieden werden. Letzte erfolgreiche Backend-Daten dürfen bei einem Folgefehler nur als solche gekennzeichnet angezeigt werden.
- **Zwischenablage:** Der kopierbare Text enthält neben den System- und Benutzerangaben den Informationsstand, den letzten erfolgreichen Abrufzeitpunkt und den Erfassungszeitpunkt.

Konkrete Komponenten, Dateinamen, Git-Stände und Prüfergebnisse werden im separaten Ergebnisdokument festgehalten.

## 6. Nichtziele

Nicht Bestandteil von BL-019 sind:

- Änderungen am fachlichen Verhalten der Mitgliederverwaltung,
- Anpassung der allgemeinen Datumsformate im Frontend,
- HTTPS-Einführung gemäß BL-026,
- allgemeine Neugestaltung der Benutzeroberfläche außerhalb der Kopfzeile und des Info-Dialogs,
- grundlegende Überarbeitung der Backend-Healthchecks,
- Änderungen an Container- oder Deployment-Architektur ohne nachgewiesene Notwendigkeit,
- eine neue lokale Datenbank oder Änderung der bestehenden Datenbankzuordnung,
- Anzeige von Zugangsdaten oder internen Verbindungsdetails,
- Freitextfeld zur Fehlerbeschreibung, automatische Fehlerübermittlung oder Logexport,
- Einführung eines nur für Administratoren verfügbaren Info-Bereichs,
- Aufbau einer neuen Infrastruktur für React-Hook- und Komponententests (BL-021),
- dauerhafte Backend-Erreichbarkeitsüberwachung oder Neugestaltung des Login-Fehlerverhaltens.

## 7. Qualitätssicherung und Abnahmekriterien

BL-019 gilt als fachlich und technisch umgesetzt, wenn:

- LOCAL, DEV und PROD anhand der tatsächlichen Backend-Informationen korrekt und eindeutig in der Kopfzeile erscheinen,
- die Anzeige von der Backend-Ausführungsumgebung und nicht von der verwendeten Datenbank abhängt,
- bei nicht erreichbarem Backend sowie unbekannten/ungültigen Umgebungswerten keine falsche Umgebung angezeigt wird,
- die Kopfzeile die abgestimmten Elemente enthält und die bestehende Navigation und Abmeldung funktionieren,
- Benutzername und Rolle in der Kopfzeile korrekt angezeigt werden,
- die Versionsnummern aus der Kopfzeile entfernt und im Info-Dialog verfügbar sind,
- der Info-Dialog für alle angemeldeten Benutzer zugänglich ist und sämtliche festgelegten Angaben enthält,
- der Frontend-Build-Zeitpunkt beim Build erzeugt und korrekt angezeigt wird,
- die Funktion „Informationen kopieren“ die verfügbaren Angaben lesbar und ohne Geheimnisse übernimmt,
- der Info-Dialog bei einem Backend-Ausfall während einer bereits geladenen und angemeldeten Sitzung nutzbar bleibt und letzte bekannte Informationen korrekt kennzeichnet,
- beim Öffnen des Info-Dialogs ein erneuter Abruf stattfindet, ohne dauerhafte Hintergrundabfragen,
- `localhost`, Proxy-Verbindungen und interne Verbindungsadressen nicht mehr irreführend in der Kopfzeile erscheinen, auch nicht in PROD,
- die Frontend-Backend-Kommunikation durch den Rückbau der Anzeige nicht beeinträchtigt wird,
- die bestehenden Frontend-Tests erfolgreich sind; automatisierte Hook- und Komponententests werden gemäß BL-021 gesondert eingeführt,
- der Frontend-Build erfolgreich durchgeführt wurde,
- die Funktion lokal und nach DEV-Deployment geprüft wurde,
- die erforderliche Dokumentation aktualisiert wurde.

**Abschlussstand (04.11.2026):** Die Implementierung wurde nach der lokalen Prüfung in DEV bereitgestellt und mit Frontend-Release `1.2.1` am 04.11.2026 in PROD produktiv gesetzt. Die ursprünglichen LOCAL-Einzelprüfungen und die Grenzen der DEV-/PROD-Prüfnachweise sind im Ergebnisdokument `BL-019-frontend-anzeige-laufzeitumgebung-ergebnis-v0.2.md` festgehalten.

**Dokumentierte Ausnahme zu FA-019-05:** Die Kopierfunktion ist implementiert und wurde lokal geprüft. Unter HTTP kann sie in DEV und PROD aufgrund der Sicherheitsanforderungen des Browsers scheitern. Die erneute Prüfung in DEV und PROD nach HTTPS-Einführung ist als verbindliches Abnahmekriterium in **BL-026** übernommen. Damit wird das ursprüngliche Abnahmekriterium zur Kopierfunktion nicht als uneingeschränkt nachgewiesen dargestellt; BL-019 wird mit dieser ausdrücklich dokumentierten Einschränkung abgeschlossen.

## 8. Dokumentationsauswirkungen

Nach Umsetzung sind auf Anpassungsbedarf zu prüfen:

- dieses BL-019-Anforderungsdokument,
- BL-019-Ergebnisdokument,
- Projektbacklog,
- Architekturentscheidungen, soweit erforderlich,
- Deployment-Dokumentation, soweit betroffen,
- `CHANGELOG.md` zum passenden Releasezeitpunkt.

## 9. Abhängigkeiten

Technische Grundlage ist BL-007 – Backend Healthchecks beziehungsweise die dort eingeführten Systeminformationen. `/api/system/info` stellt die benötigten Backend-Angaben bereits bereit.

Für LOCAL ist das Profil `local` maßgeblich. Benutzername und Rolle sollen aus dem vorhandenen Frontend-Anmeldekontext bezogen werden. Für die spätere automatisierte Prüfung der neuen React-Komponenten und des Hooks ist BL-021 maßgeblich.

## 10. Erwartetes Ergebnis und Abschlussstand

Die EMC Mitgliederverwaltung zeigt in einer aufgeräumten Kopfzeile eindeutig die tatsächlich verwendete Backend-Ausführungsumgebung LOCAL, DEV oder PROD. Alle angemeldeten Benutzer können über **Info** den technischen Stand von Frontend und Backend einschließlich beider Build-Zeitpunkte sowie ihren Benutzerkontext einsehen und für die Weitergabe an die Entwicklung kopieren.

Die irreführende `localhost`-/Proxy-Anzeige soll entfallen. Die Umsetzung verwendet möglichst vorhandene Funktionen und bleibt auf den abgestimmten Umfang von BL-019 begrenzt.

## 11. Abschlussvermerk (04.11.2026)

BL-019 ist mit dem Frontend-Release `1.2.1` produktiv umgesetzt. Die Kopfzeile zeigt die vom Backend gelieferte Laufzeitumgebung; der Info-Dialog stellt Systeminformationen für angemeldete Benutzer bereit. Die bisherige statische `localhost`-/Proxy-Anzeige wurde entfernt.

Die dokumentierte Einschränkung der Zwischenablagefunktion unter HTTP bleibt bis zur Überprüfung unter HTTPS bestehen. Ihre Prüfung in DEV und PROD ist BL-026 zugeordnet. Das Ergebnisdokument `BL-019-frontend-anzeige-laufzeitumgebung-ergebnis-v0.2.md` enthält den Umsetzungs- und Prüfstand einschließlich der Nachweisgrenzen. Weitere fachliche Änderungen an BL-019 sind damit nicht vorgesehen.
