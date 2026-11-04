# BL-019 – Frontend-Anzeige der Laufzeitumgebung

**Anforderungs-ID:** BL-019  
**Dokumentversion:** v0.2  
**Stand:** 27.10.2026  
**Status:** In Analyse – fachliche Gestaltung abgestimmt  
**Priorität:** Mittel  
**Roadmap-Bezug:** P1 – Technische Produktreife

## 1. Ziel

Das Frontend der EMC Mitgliederverwaltung soll die tatsächlich verwendete Backend-Laufzeitumgebung eindeutig anzeigen. Unterstützt werden **LOCAL**, **DEV** und **PROD**.

Verwechslungen zwischen Entwicklungs- und Produktivumgebung sollen vermieden werden. Zusätzlich sollen alle angemeldeten Benutzer über einen **Info-Dialog** die für eine Fehlermeldung an die Entwicklung benötigten Systeminformationen einsehen und kopieren können.

## 2. Ausgangssituation

Die EMC Mitgliederverwaltung besteht aus einem React/Vite-Frontend und einem Java/Spring-Boot-Backend. Es bestehen getrennte Backend-Ausführungsumgebungen LOCAL (lokal aus IntelliJ), DEV (Docker/NAS) und PROD (Docker/NAS).

Das Backend stellt seit BL-007 Betriebsinformationen über den authentifizierten Endpunkt `/api/system/info` bereit. Die Antwort enthält `backendVersion`, `environment`, `activeProfiles` und `buildTime`.

Die bestehende Frontend-Kopfzeile enthält eine Versionsanzeige und die fest kodierten Angaben `localhost` sowie `Proxy → localhost:8080`. Diese erscheinen auch in PROD und sind deshalb irreführend. Die bestehende Frontend-Version stammt aus der `package.json` und wird über Vite bereitgestellt; ein Frontend-Build-Zeitpunkt wird bislang nicht bereitgestellt.

Die Backend-Umgebung wird anhand aktiver Spring-Profile ermittelt. Für LOCAL wurde die IntelliJ-Startkonfiguration von `dev` auf `local` umgestellt und `/api/system/info` erfolgreich mit `environment: LOCAL` und `activeProfiles: ["local"]` geprüft. Das lokale Backend verwendet weiterhin die DEV-Datenbank auf dem NAS. Die Datenbankumgebung bestimmt **nicht** die Anzeige LOCAL/DEV/PROD.

Die Backend-Version stammt aus den beim Maven-Build erzeugten Build-Informationen. Nach einem erneuten Build wurde lokal `1.2.1-SNAPSHOT` erfolgreich über die API geprüft. Eine Backend-Codeänderung ist nach bisherigem Prüfstand für BL-019 nicht erforderlich.

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

Die Benutzerinformationen sollen nach Möglichkeit aus bereits verfügbaren Anmeldedaten stammen. Die genaue Frontend-Anbindung ist vor der Implementierung zu prüfen.

### FA-019-05 – Systeminformationen kopieren

Der Info-Dialog bietet eine Funktion **„Informationen kopieren“**. Sie übernimmt die angezeigten Systeminformationen in lesbarer, deutsch beschrifteter Textform in die Zwischenablage, damit sie beispielsweise in einer Fehlermeldung weitergegeben werden können.

Ein Freitextfeld, eine automatische Fehlermeldungsübermittlung und ein automatischer Logexport sind **nicht** Bestandteil von BL-019.

### FA-019-06 – Fehlerverhalten

Kann die Backend-Laufzeitumgebung nicht ermittelt werden, darf keine unbelegte oder möglicherweise falsche Umgebung angezeigt werden. Stattdessen wird die Umgebung als **„Unbekannt“** kenntlich gemacht. Nicht verfügbare Backend-Informationen werden im Info-Dialog verständlich gekennzeichnet; die verfügbaren Frontend- und Benutzerinformationen bleiben zugänglich.

Unbekannte oder ungültige API-Umgebungswerte dürfen nicht ungeprüft als LOCAL, DEV oder PROD interpretiert werden. Ein fehlgeschlagener Abruf darf die normale Bedienbarkeit der Oberfläche nicht beeinträchtigen.

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

Beim Vite-Build wird ein eindeutiger Build-Zeitpunkt erzeugt und der Anwendung zur Anzeige im Info-Dialog bereitgestellt. Er muss den **tatsächlich ausgelieferten Frontend-Build** beschreiben und darf nicht bei jedem Seitenaufruf neu entstehen. Die bestehende Frontend-Versionsnummer bleibt erhalten.

### TA-019-04 – Keine Beeinträchtigung bestehender Funktionen

Navigation, Berechtigungen, Anmeldung/Abmeldung, vorhandene Fachfunktionen und Backend-Kommunikation dürfen nicht beeinträchtigt werden. Unnötige Refactorings sind ausgeschlossen.

### TA-019-05 – Robustes Fehlerverhalten und Datenschutz

Der Info-Dialog und die Kopfzeile bleiben bei fehlgeschlagenem Abruf von `/api/system/info` nutzbar. Es werden keine Passwörter, Tokens, Session-Cookies, vollständigen Datenbank-Verbindungszeichenfolgen oder sonstigen Geheimnisse angezeigt oder kopiert.

### TA-019-06 – LOCAL-Konfiguration

Die bereits geprüfte lokale Startkonfiguration verwendet das Spring-Profil `local`, während die Datenbankverbindung über die bestehenden Umgebungsvariablen weiterhin auf die DEV-Datenbank auf dem NAS zeigen kann. Für die Umsetzung ist keine zusätzliche lokale Datenbank vorgesehen.

## 5. Noch offene technische Festlegungen

Die fachliche Gestaltung der Kopfzeile und des Info-Dialogs ist abgestimmt. Vor der Implementierung sind noch technisch zu konkretisieren:

- Einbindung von Benutzername und Rolle aus dem vorhandenen Frontend-Anmeldekontext,
- konkrete UI-Komponenten und responsive Darstellung der Kopfzeile,
- Formatierung des Frontend-Build-Zeitpunkts und der Backend-Zeitangaben,
- Darstellung und Kopiertext bei fehlenden einzelnen Systeminformationen,
- technische Umsetzung und Tests der Zwischenablagefunktion.

Diese Punkte dürfen die beschlossenen fachlichen Anforderungen nicht stillschweigend verändern.

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
- Einführung eines nur für Administratoren verfügbaren Info-Bereichs.

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
- der Info-Dialog auch bei nicht erreichbarem Backend nutzbar bleibt,
- `localhost`, Proxy-Verbindungen und interne Verbindungsadressen nicht mehr irreführend in der Kopfzeile erscheinen, auch nicht in PROD,
- die Frontend-Backend-Kommunikation durch den Rückbau der Anzeige nicht beeinträchtigt wird,
- erforderliche neue Tests und bestehende Frontend-Tests erfolgreich sind,
- der Frontend-Build erfolgreich durchgeführt wurde,
- die Funktion lokal und nach DEV-Deployment geprüft wurde,
- die erforderliche Dokumentation aktualisiert wurde.

Ein PROD-Deployment erfolgt erst nach gesonderter Entscheidung und Freigabe.

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

Die lokale Umstellung auf das Profil `local` wurde erfolgreich geprüft. Die bestehende Frontend-Authentifizierung ist Grundlage für Benutzername und Rolle im Info-Dialog; deren konkrete Nutzung ist noch technisch zu prüfen.

## 10. Erwartetes Ergebnis

Die EMC Mitgliederverwaltung zeigt in einer aufgeräumten Kopfzeile eindeutig die tatsächlich verwendete Backend-Ausführungsumgebung LOCAL, DEV oder PROD. Alle angemeldeten Benutzer können über **Info** den technischen Stand von Frontend und Backend einschließlich beider Build-Zeitpunkte sowie ihren Benutzerkontext einsehen und für die Weitergabe an die Entwicklung kopieren.

Die irreführende `localhost`-/Proxy-Anzeige ist entfernt. Die Umsetzung verwendet möglichst vorhandene Funktionen und bleibt auf den abgestimmten Umfang von BL-019 begrenzt.
