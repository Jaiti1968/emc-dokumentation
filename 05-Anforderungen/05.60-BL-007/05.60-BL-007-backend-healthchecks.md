# 05.60-BL-007-backend-healthchecks.md

Status: Geplant

Priorität: Hoch

Roadmap-Bezug: P1 – Backend Healthchecks / Spring Boot Actuator

---

# BL-007 Backend Healthchecks und Spring Boot Actuator

## 1. Ziel

Einführung eines standardisierten Healthcheck- und Betriebsinformationskonzeptes für das Backend der EMC Mitgliederverwaltung.

Das Backend soll seinen Betriebszustand maschinenlesbar bereitstellen und dadurch eine verbesserte Überwachung, Fehlererkennung und Betriebsstabilität ermöglichen.

---

# 2. Ausgangssituation

Das Backend basiert auf:

- Java 21
- Spring Boot 3
- Spring Security
- JdbcTemplate
- MariaDB

Derzeit existieren keine standardisierten Healthcheck-Endpunkte.

Eine externe Überwachung kann aktuell lediglich die generelle Erreichbarkeit des Backends prüfen.

Eine Aussage über den tatsächlichen Betriebszustand des Systems oder seiner Abhängigkeiten ist nicht möglich.

---

# 3. Motivation

Mit Abschluss der NAS-Betriebsstandardisierung wurde eine standardisierte Monitoring- und Betriebsplattform geschaffen.

Zur vollständigen Integration in dieses Betriebsmodell soll das Backend seinen Zustand aktiv bereitstellen.

Dadurch sollen insbesondere folgende Ziele erreicht werden:

- frühzeitige Fehlererkennung
- bessere Betriebsüberwachung
- standardisierte Monitoring-Schnittstellen
- vereinfachte Fehlersuche
- Grundlage für zukünftige Automatisierung
- bessere Recovery-Unterstützung

---

# 4. Fachliche Anforderungen

## FA-007-01 Technischer Healthcheck

Das Backend soll einen standardisierten technischen Healthcheck bereitstellen.

Der Healthcheck soll den allgemeinen Betriebszustand des Backends liefern.

---

## FA-007-02 Datenbank-Healthcheck

Der Healthcheck soll den Zustand der MariaDB-Anbindung berücksichtigen.

Geprüft werden soll mindestens:

- Datenbank erreichbar
- Datenbankverbindung nutzbar

---

## FA-007-03 Betriebsinformationen

Das Backend soll ausgewählte Betriebsinformationen bereitstellen.

Mögliche Informationen:

- Versionsnummer
- Build-Version
- aktive Spring-Profile
- Startzeitpunkt
- Laufzeit

---

## FA-007-04 Monitoring-Fähigkeit

Die bereitgestellten Informationen müssen für ein externes Monitoring nutzbar sein.

Insbesondere soll eine spätere Integration in Uptime Kuma möglich sein.

---

## FA-007-05 Erweiterbarkeit

Das Healthcheck-Konzept soll zukünftige Erweiterungen ermöglichen.

Beispiele:

- Mailversand
- externe Dienste
- Dateisystemprüfungen
- weitere Infrastrukturkomponenten

---

# 5. Technische Anforderungen

## TA-007-01 Spring Boot Actuator

Für die Bereitstellung standardisierter Health-Informationen soll Spring Boot Actuator verwendet werden.

---

## TA-007-02 Liveness-Prüfung

Das Backend soll eine Liveness-Prüfung unterstützen.

Ziel:

Feststellung, ob die Anwendung grundsätzlich lauffähig ist.

---

## TA-007-03 Readiness-Prüfung

Das Backend soll eine Readiness-Prüfung unterstützen.

Ziel:

Feststellung, ob die Anwendung Anfragen bearbeiten kann.

---

## TA-007-04 Datenbankintegration

Die Datenbankprüfung soll Bestandteil des Health-Konzeptes sein.

---

## TA-007-05 Dokumentierte Konfiguration

Die Konfiguration der Health-Endpunkte muss nachvollziehbar dokumentiert werden.

---

# 6. Sicherheitsanforderungen

## SA-007-01 Minimale Informationspreisgabe

Es dürfen keine sensiblen Betriebsinformationen öffentlich zugänglich gemacht werden.

---

## SA-007-02 Security Hardening

Die bestehenden Security-Hardening-Maßnahmen dürfen nicht geschwächt werden.

---

## SA-007-03 Endpunktbewertung

Für jeden bereitgestellten Actuator-Endpunkt ist zu bewerten:

- öffentlich
- authentifiziert
- deaktiviert

---

## SA-007-04 Sichere Standardkonfiguration

Nicht benötigte Actuator-Endpunkte sind deaktiviert zu halten.

---

# 7. Nichtziele

Nicht Bestandteil dieser Anforderung sind:

- Frontend-Healthchecks
- Container-Orchestrierung
- Kubernetes-Integration
- automatische Recovery-Prozesse
- Infrastrukturänderungen innerhalb EMC-INFRA

---

# 8. Dokumentationsauswirkungen

## Backend

Anpassungen zu erwarten:

- README
- Architekturübersicht
- Betriebsinformationen

---

## EMC-INFRA

Mögliche spätere Anpassungen:

- Monitoring-Dokumentation
- Uptime-Kuma-Konfiguration
- Betriebsinventar

Nur sofern sich daraus Änderungen ergeben.

---

## EMC-DOKUMENTATION

Anpassungen:

- Projektstatus
- Backlog
- Roadmap
- Ergebnisdokumentation

---

# 9. Abnahmekriterien

Die Anforderung gilt als erfüllt, wenn:

- Spring Boot Actuator integriert ist
- Health-Endpunkte verfügbar sind
- Datenbankstatus berücksichtigt wird
- Security-Konzept umgesetzt ist
- Monitoring-Nutzung möglich ist
- Dokumentation aktualisiert wurde
- Ergebnisdokument erstellt wurde

---

# 10. Abhängigkeiten

Keine fachlichen Abhängigkeiten.

Technische Voraussetzung:

- bestehendes Spring Boot Backend
- bestehende MariaDB-Anbindung

---

# 11. Risiken

Gering.

Mögliche Risiken:

- unbeabsichtigte Offenlegung von Betriebsinformationen
- Konflikte mit bestehender Security-Konfiguration
- Fehlkonfiguration von Actuator-Endpunkten

Diese Risiken sind während der Umsetzung explizit zu prüfen.

---

# 12. Erwartetes Ergebnis

Nach Umsetzung verfügt das Backend über:

- standardisierte Health-Endpunkte
- Datenbanküberwachung
- Betriebsinformationen
- Monitoring-fähige Schnittstellen

und ist damit besser in die standardisierte Betriebsplattform integrierbar.

---

Anforderungs-ID: BL-007

Titel: Backend Healthchecks und Spring Boot Actuator

Status: Geplant

Priorität: Hoch
