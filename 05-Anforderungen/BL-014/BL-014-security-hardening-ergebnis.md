# BL-014 Security Hardening – Ergebnisdokument

## Status

**Abgeschlossen**

## Abschlussdatum

19.06.2026

---

# Ziel der Anforderung

Ziel von BL-014 war die Überprüfung und pragmatische Härtung der Sicherheitsmechanismen der EMC Mitgliederverwaltung.

Der Fokus lag bewusst nicht auf einer vollständigen Enterprise-Security-Architektur, sondern auf Maßnahmen mit hohem Sicherheitsnutzen bei gleichzeitig geringem Implementierungs- und Wartungsaufwand.

Die Umsetzung sollte zum Charakter des Projekts als Einzelentwickler- und Vereinsanwendung passen.

---

# Durchgeführte Analyse

Im Rahmen der Anforderung wurden untersucht:

- Authentifizierungsverfahren
- Rollen- und Berechtigungsmodell
- Session-Verwaltung
- Fehlerbehandlung
- Benutzerverwaltung
- Passwortprozesse
- Cookie-Konfiguration
- Betriebsumgebung DEV / PROD
- zukünftige Anforderungen für öffentliche Bereitstellung

Die Analyse ergab, dass die vorhandene Sicherheitsarchitektur bereits auf einem soliden Fundament basiert.

Insbesondere waren bereits umgesetzt:

- Spring Security
- rollenbasierte Zugriffskontrolle
- Passwort-Hashing mit BCrypt
- serverseitige Sessions
- zentrale Fehlerbehandlung
- CORS-Konfiguration
- Trennung von DEV und PROD
- dedizierte Datenbankbenutzer

---

# Umgesetzte Maßnahmen

## E-024 Schutz des letzten aktiven Administrators

### Ziel

Verhindern, dass die Anwendung in einen Zustand ohne administrativen Benutzer überführt werden kann.

### Umsetzung

Erweiterung von:

- UserRepository
- AdminUserService

Implementierte Schutzmechanismen:

- letzter aktiver ADMIN kann nicht deaktiviert werden
- letzter aktiver ADMIN kann nicht auf EDITOR oder VIEWER herabgestuft werden

### Ergebnis

Die Anwendung stellt dauerhaft sicher, dass mindestens ein aktiver Administrator vorhanden bleibt.

---

## E-025 Defensive Fehlerbehandlung

### Ziel

Verhindern der Offenlegung technischer Implementierungsdetails gegenüber Clients.

### Umsetzung

Die generische Fehlerbehandlung wurde angepasst.

Unerwartete technische Fehler liefern nun eine neutrale Fehlermeldung:

```text
Ein unerwarteter Fehler ist aufgetreten.
```

Technische Details verbleiben ausschließlich im Backend-Log.

Zusätzlich wurde die Behandlung fachlicher HTTP-Fehler verbessert.

Beispiele:

- 404 Not Found
- 409 Conflict

werden mit fachlicher Fehlermeldung an den Client zurückgegeben.

### Ergebnis

Verbesserter Schutz vor Information Leakage bei gleichzeitig guter Benutzerführung.

---

## E-026 Session- und Cookie-Governance

### Ziel

Explizite Definition des Session- und Cookie-Verhaltens.

### Umsetzung

Konfiguration:

```properties
server.servlet.session.timeout=30m
server.servlet.session.cookie.http-only=true
server.servlet.session.cookie.same-site=lax
```

Zusätzlich:

```properties
server.servlet.session.cookie.secure=true
```

im PROD-Profil.

### Ergebnis

- definierter Session Timeout
- HttpOnly-Cookies
- SameSite-Schutz
- HTTPS-Cookie-Schutz für PROD

---

# Nicht umgesetzte Maßnahmen

## K-01 CSRF-Schutz

Bewertung:

Aktuell nicht erforderlich.

Begründung:

- Anwendung wird ausschließlich im Heimnetz betrieben
- externer Zugriff nur über VPN
- keine öffentliche Bereitstellung

Status:

```text
bewusst zurückgestellt
```

Eine spätere Aktivierung bei öffentlicher Bereitstellung bleibt vorgesehen.

---

## K-02 Passwort vergessen Prozess

Bewertung:

Fachlich sinnvoll, derzeit jedoch nicht notwendig.

Vorgesehener zukünftiger Ansatz:

```text
Benutzer meldet Passwort vergessen
↓
Kennzeichen am Benutzerkonto
↓
ADMIN erhält Hinweis beim Login
↓
ADMIN vergibt neues Passwort
```

Status:

```text
als Folgeanforderung vorgesehen
```

---

# Qualitätssicherung

## Testerweiterung

Neu erstellt:

```text
AdminUserServiceTest
```

Abgesichert:

- Schutz letzter ADMIN
- Rollenänderungen
- Aktivierungs-/Deaktivierungslogik

---

## Gesamter Teststand

```text
73 Tests
0 Failures
0 Errors
0 Skipped
```

---

# Deployment und Validierung

## DEV Deployment

Version:

```text
Backend 1.1.3-SNAPSHOT
```

Deployment erfolgreich durchgeführt.

---

## Fachliche Validierung

Geprüft:

### Letzter ADMIN

Test:

```text
ADMIN → EDITOR
```

Ergebnis:

```text
HTTP 409 Conflict
```

Frontend zeigt korrekt:

```text
Der letzte aktive Administrator darf nicht deaktiviert oder herabgestuft werden.
```

---

### Session Timeout

Test:

```text
Anmeldung
↓
Inaktivität über Timeout hinaus
↓
erneuter Zugriff
```

Ergebnis:

```text
erneute Anmeldung erforderlich
```

Die serverseitige Session wird ordnungsgemäß beendet.

---

# Dokumentationsanpassungen

Aktualisiert:

- README.md
- CHANGELOG.md

Dokumentiert wurden:

- Security Hardening
- Session Governance
- Cookie Governance
- Fehlerbehandlung
- aktueller Teststand

---

# Entscheidungen

Im Rahmen von BL-014 wurden folgende Architekturentscheidungen getroffen:

- E-024 Schutz letzter ADMIN
- E-025 Defensive Fehlerbehandlung
- E-026 Session Timeout konfigurieren

Die Entscheidungen wurden in der Projektdokumentation übernommen.

---

# Gesamtbewertung

BL-014 wurde erfolgreich abgeschlossen.

Die Sicherheitsarchitektur der EMC Mitgliederverwaltung wurde gezielt an den aktuellen Projektbedarf angepasst.

Die umgesetzten Maßnahmen erhöhen die Betriebssicherheit und Robustheit der Anwendung ohne zusätzliche Infrastruktur oder unnötige Komplexität einzuführen.

Die Lösung bleibt vollständig kompatibel mit dem pragmatischen Entwicklungs- und Betriebsmodell des Projekts.
