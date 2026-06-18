# BL-014 Maßnahmenkatalog mit Priorisierung

Status: Entwurf

Anforderung: BL-014 Security Hardening

Stand: 18.06.2026

---

# 1. Zielsetzung

Dieses Dokument fasst die Ergebnisse der Security-Analyse im Rahmen von BL-014 zusammen.

Ziel ist die Bewertung identifizierter Risiken sowie die Priorisierung möglicher Sicherheitsmaßnahmen nach Aufwand, Nutzen und Relevanz für den Produktivbetrieb der EMC Mitgliederverwaltung.

Die Bewertung erfolgt bewusst pragmatisch und orientiert sich an den Anforderungen eines Ein-Personen-Projekts.

Nicht Ziel ist die Einführung von Enterprise-Sicherheitsmechanismen.

---

# 2. Priorisierungssystem

## Muss

Sicherheitsrelevante Schwachstellen oder Risiken mit unmittelbarem Handlungsbedarf.

Merkmale:

- relevantes Sicherheitsrisiko
- geringer bis mittlerer Umsetzungsaufwand
- hoher Sicherheitsgewinn

---

## Sollte

Sinnvolle Verbesserungen mit erkennbarem Sicherheitsgewinn.

Merkmale:

- mittleres Risiko
- mittlerer Aufwand
- Verbesserung von Robustheit und Wartbarkeit

---

## Kann

Optionale Verbesserungen.

Merkmale:

- geringes Risiko
- Komfort- oder Governance-Themen
- keine unmittelbare Notwendigkeit

---

# 3. Muss-Maßnahmen

## M-01 Entfernung von Secrets aus Repositories

### Feststellung

Im Backend wurden Konfigurationsdateien identifiziert, die Zugangsdaten enthalten.

Betroffen:

```text
.idea/workspace.xml

application-test.properties
```

### Risiko

- Offenlegung von Zugangsdaten
- versehentliche Weitergabe bei Repository-Freigaben
- langfristige Credential-Kompromittierung

### Maßnahme

- Entfernung aller Zugangsdaten aus dem Repository
- Nutzung von Environment Variablen
- Rotation betroffener Passwörter

### Priorität

MUSS

---

## M-02 Schutz des letzten ADMIN-Kontos

### Feststellung

Aktuell ist nicht sichergestellt, dass mindestens ein aktiver ADMIN erhalten bleibt.

Mögliche Szenarien:

- letzter ADMIN wird deaktiviert
- letzter ADMIN wird auf EDITOR herabgestuft
- versehentliche Selbstsperrung

### Risiko

Vollständiger Verlust administrativer Zugriffe.

### Maßnahme

Verhindern von:

- Deaktivierung des letzten aktiven ADMIN
- Herabstufung des letzten aktiven ADMIN

Optional zusätzlich:

- Schutz vor Selbstdeaktivierung
- Schutz vor Selbstherabstufung

### Priorität

MUSS

---

## M-03 Härtung generischer Fehlerbehandlung

### Feststellung

Technische Fehlermeldungen werden teilweise an den Client zurückgegeben.

### Risiko

Offenlegung interner Implementierungsdetails.

Beispiele:

- SQL-Fehler
- technische Exceptions
- interne Klasseninformationen

### Maßnahme

Rückgabe neutraler Fehlermeldungen.

Beispiel:

```text
Ein unerwarteter Fehler ist aufgetreten.
```

Technische Details ausschließlich im Server-Log.

### Priorität

MUSS

---

# 4. Sollte-Maßnahmen

## S-01 Session-Governance definieren

### Feststellung

Session-Verhalten ist technisch vorhanden, aber nicht explizit dokumentiert.

Zu definieren:

- Session Timeout
- Logout-Verhalten
- Session Invalidierung
- Cookie-Regeln

### Ziel

Nachvollziehbare Sicherheitsregeln.

### Priorität

SOLLTE

---

## S-02 Cookie-Härtung prüfen

### Zu prüfen

```text
HttpOnly

Secure

SameSite
```

### Ziel

Schutz vor:

- Session Hijacking
- CSRF-Risiken
- Browser-Missbrauch

### Priorität

SOLLTE

---

## S-03 CSRF-Bewertung dokumentieren

### Feststellung

CSRF-Schutz ist aktuell deaktiviert.

### Bewertung erforderlich

Prüfung:

- tatsächliches Risiko
- Auswirkungen der aktuellen Architektur
- Aufwand einer Aktivierung

### Ergebnis

Bewusste Architekturentscheidung dokumentieren.

### Priorität

SOLLTE

---

## S-04 Frontend-Rollenschutz ergänzen

### Feststellung

Administrative Navigation ist geschützt.

Administrative Routen sind jedoch nicht vollständig rollenbasiert abgesichert.

### Risiko

Kein Sicherheitsproblem auf Backend-Ebene.

Mögliche UI-Inkonsistenzen.

### Maßnahme

Rollenbasierte Route Guards ergänzen.

### Priorität

SOLLTE

---

# 5. Kann-Maßnahmen

## K-01 Passwortwechsel beim Erstlogin

### Ziel

Vermeidung dauerhaft administrativ gesetzter Passwörter.

### Bewertung

Sicherheitsgewinn vorhanden.

Für den aktuellen Projektumfang nicht zwingend erforderlich.

### Priorität

KANN

---

## K-02 Passwort-Reset-Verfahren

### Ziel

Selbstständige Wiederherstellung von Benutzerkonten.

### Bewertung

Aktuell genügt administratives Passwortsetzen.

### Priorität

KANN

---

## K-03 Security Logging

### Ziel

Nachvollziehbarkeit kritischer Administratoraktionen.

Beispiele:

- Benutzer angelegt
- Benutzer deaktiviert
- Passwort geändert
- Rolle geändert

### Bewertung

Nützlich für spätere Auditierung.

### Priorität

KANN

---

## K-04 Erweiterte Passwortregeln

### Beispiele

- Mindestlänge > 8
- Sonderzeichenpflicht
- Zahlenpflicht

### Bewertung

Für das aktuelle Projekt begrenzter Mehrwert.

Passphrasen werden bevorzugt.

### Priorität

KANN

---

# 6. Nicht empfohlene Maßnahmen

Folgende Maßnahmen werden derzeit nicht empfohlen.

## Identity Provider

Beispiele:

- Keycloak
- OAuth
- OpenID Connect

Begründung:

Überdimensioniert für den Projektumfang.

---

## Multi-Faktor-Authentifizierung

Begründung:

Zusätzliche Komplexität ohne ausreichenden Nutzen.

---

## SIEM / Security Monitoring

Begründung:

Nicht verhältnismäßig.

---

## Komplexe Passwort-Compliance

Begründung:

Passphrasen sind ausreichend.

---

## Externe Security Audits

Begründung:

Kein angemessenes Kosten-Nutzen-Verhältnis.

---

# 7. Vorläufige Empfehlung

Direkte Umsetzung innerhalb von BL-014:

```text
M-01 Secrets entfernen

M-02 Letzten ADMIN schützen

M-03 Fehlerbehandlung härten
```

Dokumentations- und Governance-Ergebnis:

```text
S-01 Session-Governance

S-02 Cookie-Härtung

S-03 CSRF-Entscheidung
```

Mögliche Folgeanforderungen:

```text
Security Logging

Admin Recovery

Passwort-Reset

Erweiterte Auditierung
```

---

# 8. Zwischenfazit

Die Sicherheitsbasis der EMC Mitgliederverwaltung wird insgesamt als gut bewertet.

Die identifizierten Risiken konzentrieren sich überwiegend auf:

- Governance
- Recoveryfähigkeit
- defensive Absicherung

Kritische Architekturprobleme wurden nicht festgestellt.

Die priorisierten Muss-Maßnahmen liefern einen hohen Sicherheitsgewinn bei geringem Implementierungsaufwand.
