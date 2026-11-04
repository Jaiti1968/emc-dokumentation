# BL-019 – Frontend-Anzeige der Laufzeitumgebung

**Anforderungs-ID:** BL-019  
**Dokumentversion:** v0.1  
**Stand:** 27.10.2026  
**Status:** In Analyse  
**Priorität:** Mittel  
**Roadmap-Bezug:** P1 – Technische Produktreife

## 1. Ziel

Das Frontend der EMC Mitgliederverwaltung soll die tatsächlich verwendete Backend-Laufzeitumgebung eindeutig anzeigen. Unterstützt werden **LOCAL**, **DEV** und **PROD**.

Insbesondere sollen Verwechslungen zwischen Entwicklungs- und Produktivumgebung vermieden werden. Die Anzeige dient auch der Kontrolle nach Deployments und der Fehlersuche im Betrieb.

## 2. Ausgangssituation

Die EMC Mitgliederverwaltung besteht aus einem React/Vite-Frontend und einem Java/Spring-Boot-Backend. Es bestehen getrennte Laufzeitumgebungen LOCAL, DEV und PROD.

Das Backend stellt seit der Umsetzung von BL-007 Betriebsinformationen über `/api/system/info` bereit. Das Frontend verfügt bereits über eine Versionsanzeige.

In der derzeitigen Oberfläche werden außerdem technische Angaben wie `localhost` und `Proxy → localhost:8080` angezeigt. Diese Anzeige ist nachweislich auch in **PROD** sichtbar. Dort ist sie irreführend und für die reguläre Nutzung ungeeignet.

Die tatsächliche Antwortstruktur des Endpunkts und die Implementierung der vorhandenen Frontend-Anzeigen sind noch zu prüfen.

## 3. Fachliche Anforderungen

### FA-019-01 – Anzeige der Laufzeitumgebung

Das Frontend muss die aktuell verwendete Backend-Laufzeitumgebung eindeutig anzeigen. Die Werte LOCAL, DEV und PROD müssen unterscheidbar sein.

### FA-019-02 – Verlässliche Informationsquelle

Maßgeblich ist die tatsächlich aktive Backend-Laufzeitumgebung. Die Anzeige darf nicht ausschließlich aus der Frontend-Buildkonfiguration oder der aufgerufenen URL abgeleitet werden.

### FA-019-03 – Integration in die Benutzeroberfläche

Die Laufzeitumgebung soll an einer geeigneten, gut erkennbaren Stelle angezeigt werden. Die bestehende Versionsanzeige ist vorrangig auf eine Erweiterung zu prüfen.

Eine mögliche Darstellung ist:

`FE v1.2.0 | BE v1.2.0 | DEV`

Dies ist noch keine Festlegung auf Position oder Gestaltung.

### FA-019-04 – Fehlerverhalten

Kann die Backend-Laufzeitumgebung nicht ermittelt werden, darf keine unbelegte oder möglicherweise falsche Umgebung angezeigt werden. Die konkrete Darstellung dieses Zustands wird nach der Bestandsaufnahme festgelegt.

### FA-019-05 – Deutschsprachige Benutzeroberfläche

Neue Beschriftungen und Fehlermeldungen müssen zur deutschsprachigen Benutzeroberfläche passen. Die technischen Kennungen LOCAL, DEV und PROD bleiben unverändert.

### FA-019-06 – Rückbau der bisherigen technischen Verbindungsanzeige

Die bisherige Anzeige technischer Verbindungsinformationen ist aus der regulären Benutzeroberfläche zu entfernen. Das betrifft insbesondere `localhost`, `Proxy → localhost:8080` sowie vergleichbare Angaben zu internen Adressen, Ports oder Proxy-Verbindungen.

An ihre Stelle tritt die im Rahmen von BL-019 festgelegte Anzeige der tatsächlichen Backend-Laufzeitumgebung.

Der Rückbau betrifft die **sichtbare Anzeige**, nicht die bestehenden technischen Mechanismen zur Kommunikation zwischen Frontend und Backend.

Ob ausgewählte technische Informationen künftig in einem gesonderten, nur für Administratoren sichtbaren Menüpunkt **„Info“** angeboten werden, ist noch offen. Diese Möglichkeit ist keine verbindliche Anforderung der v0.1.

Die Entfernung der Anzeige ist eine Bereinigung der Benutzeroberfläche und ersetzt keine technischen Sicherheitsmaßnahmen.

## 4. Technische Anforderungen

### TA-019-01 – Nutzung bestehender Schnittstellen

Vorrangig ist der bestehende Endpunkt `/api/system/info` zu verwenden. Vor der Implementierung sind seine tatsächliche Antwortstruktur und die bisherige Frontend-Anbindung zu prüfen.

### TA-019-02 – Wiederverwendung vorhandener Funktionen

Die vorhandene Versionsanzeige und bestehende Mechanismen zum Abruf von Systeminformationen sollen nach Möglichkeit wiederverwendet werden. Zusätzliche technische Strukturen sind nur bei nachgewiesenem Bedarf einzuführen.

### TA-019-03 – Keine Beeinträchtigung bestehender Funktionen

Die Umsetzung darf bestehende Frontend-Funktionen und die Kommunikation mit dem Backend nicht beeinträchtigen. Unabhängige fachliche Änderungen und unnötige Refactorings sind ausgeschlossen.

### TA-019-04 – Robustes Fehlerverhalten

Ein fehlgeschlagener Abruf der Systeminformationen darf nicht zum Ausfall der Benutzeroberfläche führen. Eine möglicherweise falsche Umgebungsangabe ist zu vermeiden.

## 5. Noch offene Festlegungen

Vor der Implementierung sind gemeinsam zu entscheiden:

- Position und dauerhafte Sichtbarkeit der Umgebungsanzeige
- Gestaltung von LOCAL, DEV und PROD, einschließlich möglicher Farbunterscheidung
- Anzeige bei fehlenden, unbekannten oder ungültigen Umgebungswerten
- Verhalten bei nicht erreichbarem Backend
- konkrete Integration in die bestehende Versionsanzeige
- mögliche spätere Bereitstellung ausgewählter technischer Informationen unter „Info“ und deren Beschränkung auf Administratoren

Insbesondere die mögliche Admin-Informationsseite wird nicht ohne gesonderte fachliche Entscheidung umgesetzt.

## 6. Nichtziele

Nicht Bestandteil von BL-019 sind:

- Änderungen am fachlichen Verhalten der Mitgliederverwaltung
- Anpassung der Datumsformate im Frontend
- HTTPS-Einführung gemäß BL-026
- allgemeine Neugestaltung der Benutzeroberfläche
- grundlegende Überarbeitung der Backend-Healthchecks
- Änderungen an Container- oder Deployment-Architektur ohne nachgewiesene Notwendigkeit
- Umsetzung eines Admin-Menüpunkts „Info“ ohne gesonderte Freigabe

## 7. Qualitätssicherung und Abnahmekriterien

BL-019 gilt als fachlich und technisch umgesetzt, wenn:

- LOCAL, DEV und PROD anhand der tatsächlichen Backend-Informationen korrekt angezeigt werden
- bei nicht erreichbarem Backend keine falsche Umgebung angezeigt wird
- die bestehende Versionsanzeige weiterhin funktioniert
- `localhost`, Proxy-Verbindungen und interne Verbindungsadressen nicht mehr in der regulären Benutzeroberfläche angezeigt werden, auch nicht in PROD
- die Entfernung dieser Anzeige die Frontend-Backend-Kommunikation nicht beeinträchtigt
- erforderliche zusätzliche Tests und bestehende Frontend-Tests erfolgreich sind
- der Frontend-Build erfolgreich durchgeführt wird
- die Funktion nach dem DEV-Deployment geprüft wurde
- die erforderliche Dokumentation aktualisiert wurde

Ein PROD-Deployment erfolgt erst nach gesonderter Entscheidung und Freigabe.

## 8. Dokumentationsauswirkungen

Nach Umsetzung sind auf Anpassungsbedarf zu prüfen:

- dieses BL-019-Anforderungsdokument
- BL-019-Ergebnisdokument
- Projektbacklog
- Architekturentscheidungen, soweit erforderlich
- Deployment-Dokumentation, soweit betroffen
- `CHANGELOG.md` zum passenden Releasezeitpunkt

## 9. Abhängigkeiten

Technische Grundlage ist BL-007 – Backend Healthchecks. Voraussetzung ist, dass `/api/system/info` geeignete Informationen über die aktive Backend-Laufzeitumgebung bereitstellt.

## 10. Erwartetes Ergebnis

Die EMC Mitgliederverwaltung zeigt die tatsächlich verwendete Backend-Laufzeitumgebung eindeutig an. Die bisherige irreführende Anzeige von `localhost` und Proxy-Verbindungsinformationen ist aus der regulären Benutzeroberfläche entfernt.

Die Umsetzung verwendet nach Möglichkeit bestehende Funktionen und bleibt auf den abgestimmten Umfang von BL-019 begrenzt.
