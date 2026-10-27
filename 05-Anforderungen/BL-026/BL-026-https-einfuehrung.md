# BL-026 – HTTPS-Einführung für die EMC Mitgliederverwaltung

**Datei:** `BL-026-https-einfuehrung.md`

**Ablagepfad:** `05-Anforderungen/BL-026/BL-026-https-einfuehrung.md`

**Stand:** 27.10.2026

**Status:** Offen

**Priorität:** Mittel

**Roadmap-Bezug:** P1 Technische Produktreife

---

# Inhaltsverzeichnis

1. Ausgangslage
2. Zielsetzung
3. Fachliche und technische Anforderungen
4. Sicherheitsanforderungen
5. DEV- und PROD-Umgebungen
6. Vorgehensweise
7. Abgrenzung
8. Abnahmekriterien
9. Dokumentationsanforderungen
10. Abhängigkeiten und Referenzen

---

# 1. Ausgangslage

Die EMC Mitgliederverwaltung wird derzeit innerhalb des internen Netzwerks über HTTP betrieben.

Die Anwendung besteht aus einem React/Vite-Frontend und einem Spring-Boot-Backend. Beide Komponenten werden containerbasiert auf der bestehenden NAS-Infrastruktur betrieben.

Für Entwicklung und produktiven Betrieb existieren getrennte DEV- und PROD-Umgebungen.

Im Rahmen der Produktivsetzung von Release v1.2.0 am 26.10.2026 wurde festgestellt, dass die bisherige Konfiguration des Session-Cookie-Secure-Flags mit dem HTTP-basierten PROD-Betrieb nicht vereinbar ist.

Daher wurde für PROD vorübergehend folgende Einstellung verwendet:

```properties
server.servlet.session.cookie.secure=false
```

Diese Abweichung von der ursprünglichen Cookie-Governance ist durch Entscheidung E-029 dokumentiert.

Der HTTP-Betrieb soll künftig vollständig durch HTTPS abgelöst werden.

---

# 2. Zielsetzung

Ziel von BL-026 ist die vollständige Umstellung der EMC Mitgliederverwaltung auf HTTPS.

Nach Abschluss der Umsetzung sollen sämtliche regulären Zugriffe auf die Anwendung verschlüsselt erfolgen.

Insbesondere sollen folgende Ziele erreicht werden:

- HTTPS als verbindlicher Zugriffsstandard
- verschlüsselte Kommunikation zwischen Browser und Anwendung
- sichere Übertragung von Anmeldedaten und Session-Cookies
- Aktivierung des Secure-Flags für Session-Cookies
- Vermeidung unverschlüsselter Anwendungszugriffe
- zuverlässiger Betrieb in DEV und PROD
- nachvollziehbare und wartbare Zertifikatsverwaltung
- Integration in die bestehende NAS- und Docker-Infrastruktur

---

# 3. Fachliche und technische Anforderungen

## 3.1 HTTPS-Zugriff

Frontend und Backend-API müssen über HTTPS erreichbar sein.

Dabei ist sicherzustellen, dass die Anwendung einschließlich Anmeldung, Navigation, Datenpflege und Reporting vollständig funktioniert.

## 3.2 TLS-Architektur

Vor der Umsetzung ist eine geeignete TLS-Architektur festzulegen.

Zu prüfen sind insbesondere:

- Nutzung eines Reverse Proxys
- HTTPS-Terminierung auf der NAS-Infrastruktur
- Weiterleitung an die bestehenden Docker-Container
- interne Kommunikation zwischen Proxy und Backend
- Auswirkungen auf bestehende Netzwerk- und Portkonfigurationen

Eine direkte TLS-Konfiguration in jedem Anwendungscontainer ist nicht zwingend erforderlich.

Die endgültige Architektur wird im Rahmen der Konzeptphase entschieden.

## 3.3 Zertifikatsverwaltung

Es ist eine geeignete Zertifikatslösung für den internen Betrieb auszuwählen.

Zu berücksichtigen sind:

- Vertrauenswürdigkeit der Zertifikate auf den verwendeten Endgeräten
- Zertifikatsgültigkeit
- Erneuerung und Ablaufüberwachung
- DNS-Namen beziehungsweise interne Adressierung
- Verwaltungs- und Wartungsaufwand
- Wiederherstellbarkeit nach einem Systemausfall

## 3.4 HTTP-Ablösung

Nach erfolgreicher Umstellung dürfen keine unverschlüsselten regulären Zugriffe auf die Anwendung mehr möglich sein.

Bestehende HTTP-Aufrufe sind entweder auf HTTPS umzuleiten oder zu unterbinden.

## 3.5 Anwendungsintegration

Folgende Funktionen müssen unter HTTPS weiterhin ordnungsgemäß arbeiten:

- Benutzeranmeldung und Abmeldung
- Session-Wiederherstellung
- Session-Timeout
- REST-API-Kommunikation
- Mitgliederverwaltung
- Reporting
- PDF-Downloads
- Versions- und Systeminformationen

---

# 4. Sicherheitsanforderungen

## 4.1 Session-Cookies

Nach erfolgreicher HTTPS-Umstellung gilt für PROD:

```properties
server.servlet.session.cookie.secure=true
```

Die bestehenden Sicherheitsfestlegungen bleiben erhalten:

- HttpOnly
- SameSite=Lax
- serverseitige Session-Verwaltung
- Session-Timeout von 30 Minuten

## 4.2 Transportverschlüsselung

Die Kommunikation zwischen Browser und HTTPS-Endpunkt muss durch eine geeignete TLS-Konfiguration geschützt werden.

Die konkrete TLS-Konfiguration wird im technischen Konzept festgelegt.

## 4.3 Netzwerkzugriff

Die Anwendung bleibt auf die vorgesehenen internen beziehungsweise gesicherten Zugangswege beschränkt.

Die Einführung von HTTPS darf nicht unbeabsichtigt zu einer öffentlichen Erreichbarkeit der Anwendung führen.

## 4.4 Weitere Sicherheitsaspekte

Im Rahmen der Umsetzung sind insbesondere zu prüfen:

- Mixed-Content-Probleme
- Cookie-Übertragung
- Weiterleitungsverhalten
- Proxy-Header
- Zertifikatsvertrauen
- Auswirkungen auf die bestehende Authentifizierung

---

# 5. DEV- und PROD-Umgebungen

Die bestehende Trennung zwischen DEV und PROD bleibt erhalten.

Beide Umgebungen sind bei der HTTPS-Einführung zu berücksichtigen.

Die Umstellung erfolgt grundsätzlich in folgender Reihenfolge:

1. Technisches Konzept erstellen
2. HTTPS in DEV einrichten
3. Funktions- und Sicherheitstests durchführen
4. Erkenntnisse dokumentieren
5. PROD-Umstellung vorbereiten
6. HTTPS in PROD aktivieren
7. Produktive Funktionsprüfung durchführen

Für die PROD-Umstellung muss ein geeignetes Rückfallverfahren vorgesehen werden.

---

# 6. Vorgehensweise

BL-026 wird in mehrere Arbeitsschritte gegliedert.

## 6.1 Bestandsaufnahme

Analyse der bestehenden Infrastruktur, insbesondere:

- NAS-Konfiguration
- Docker-Netzwerke
- Frontend- und Backend-Container
- Portainer-Stacks
- Reverse-Proxy-Möglichkeiten
- DNS- und Zertifikatsverwaltung
- Monitoring und Healthchecks

## 6.2 Technisches Konzept

Festlegung von:

- HTTPS-Architektur
- Zertifikatsstrategie
- Zugriffsadressen
- Weiterleitungen
- Konfigurationsänderungen
- Teststrategie
- Rollback-Verfahren

## 6.3 DEV-Umsetzung

Einrichtung und Prüfung der HTTPS-Konfiguration in der Entwicklungsumgebung.

## 6.4 PROD-Umsetzung

Kontrollierte Übernahme der erprobten Lösung in den produktiven Betrieb.

## 6.5 Abschluss

Dokumentation der Umsetzung, Testergebnisse und verbleibenden Betriebsanforderungen.

---

# 7. Abgrenzung

Nicht Bestandteil von BL-026 sind:

- öffentliche Bereitstellung der EMC Mitgliederverwaltung im Internet
- Einführung einer neuen Benutzer- oder Rollenverwaltung
- grundlegende Änderung des Authentifizierungsverfahrens
- Migration auf eine andere Hosting-Plattform
- fachliche Erweiterungen der Mitgliederverwaltung
- Einführung zusätzlicher Reporting-Funktionen

---

# 8. Abnahmekriterien

BL-026 gilt als erfolgreich umgesetzt, wenn:

- Frontend und Backend-API über HTTPS erreichbar sind
- die verwendeten Zertifikate auf den vorgesehenen Clients als vertrauenswürdig akzeptiert werden
- reguläre HTTP-Zugriffe umgeleitet oder unterbunden werden
- die Anmeldung unter HTTPS zuverlässig funktioniert
- Session-Cookies mit aktiviertem Secure-Flag übertragen werden
- Session-Timeout und Abmeldung weiterhin funktionieren
- die bestehenden Fachfunktionen erfolgreich geprüft wurden
- Reporting und PDF-Downloads funktionieren
- DEV und PROD erfolgreich umgestellt wurden
- Monitoring und Healthchecks angepasst und funktionsfähig sind
- ein dokumentiertes Recovery- beziehungsweise Rollback-Verfahren vorliegt
- die technische Betriebsdokumentation aktualisiert wurde

---

# 9. Dokumentationsanforderungen

Die technische Umsetzung ist in EMC-INFRA als maßgeblicher technischer Source of Truth zu dokumentieren.

Zu aktualisieren sind, soweit betroffen:

- Infrastruktur- und Stack-Dokumentation
- Deployment-Handbuch
- Reverse-Proxy- und Zertifikatskonfiguration
- Monitoring-Dokumentation
- Backup- und Recovery-Anleitungen
- Sicherheits- und Betriebsstandards

In der Produkt- und Projektdokumentation werden die fachlichen Anforderungen, Entscheidungen und Umsetzungsergebnisse festgehalten.

---

# 10. Abhängigkeiten und Referenzen

**Anforderungen:**

- BL-014 – Security Hardening
- BL-026 – HTTPS-Einführung für die EMC Mitgliederverwaltung

**Entscheidungen:**

- E-013 – DEV- und PROD-Betrieb werden dauerhaft getrennt geführt
- E-026 – Session Timeout und Cookie-Governance
- E-029 – Session-Cookie-Secure-Flag abhängig vom HTTPS-Betrieb

**Technische Grundlagen:**

- EMC-INFRA
- Backend- und Frontend-Repositories
- Deployment-Dokumentation
- Release v1.2.0 vom 26.10.2026

---

# Ergebnis

BL-026 beschreibt die geplante Ablösung des bisherigen HTTP-Betriebs durch HTTPS.

Die konkrete technische Lösung wird vor der Umsetzung analysiert und festgelegt.

Die Anforderung bleibt bis zur erfolgreichen Umsetzung und Abnahme im Status **Offen**.
