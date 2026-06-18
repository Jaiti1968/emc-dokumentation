# BL-014 – Security Hardening

**Status:** Offen

**Roadmap-Bezug:** P1 Technische Produktreife

**Priorität:** Hoch

---

# 1. Zielsetzung

Ziel von BL-014 ist die Überprüfung und gezielte Härtung der bestehenden Sicherheitsmechanismen der EMC Mitgliederverwaltung.

Die Anforderung baut auf den bereits umgesetzten Authentifizierungs-, Autorisierungs- und Session-Mechanismen auf und soll verbleibende Sicherheitsrisiken identifizieren sowie angemessene Schutzmaßnahmen definieren.

Dabei soll bewusst ein pragmatischer Ansatz verfolgt werden, der dem Charakter eines Ein-Personen-Projekts entspricht und keine unnötige Komplexität einführt.

---

# 2. Ausgangssituation

Bereits umgesetzt sind:

* Session-basierte Authentifizierung
* Spring Security Integration
* Rollenmodell (ADMIN, EDITOR, VIEWER)
* Benutzerverwaltung
* Passwortänderung durch Administrator
* Aktiv/Inaktiv-Schaltung von Benutzern
* Session Restore
* Logout
* Rollenbasierte Navigation im Frontend
* Rollenbasierte Autorisierung im Backend
* Backend Security Tests
* Frontend Authentifizierungslogik
* Healthchecks und Monitoring

Die Sicherheitsbasis wird insgesamt als solide bewertet.

Eine systematische Überprüfung verbleibender Sicherheitsaspekte wurde jedoch bisher nicht durchgeführt.

---

# 3. Ziele der Analyse

Im Rahmen von BL-014 sollen folgende Fragestellungen untersucht werden:

* Welche Sicherheitsmechanismen sind bereits ausreichend umgesetzt?
* Welche Risiken bestehen aktuell noch?
* Welche Maßnahmen liefern den größten Sicherheitsgewinn bei vertretbarem Aufwand?
* Welche Maßnahmen sind für den Produktivbetrieb tatsächlich erforderlich?
* Welche Maßnahmen würden unnötige Komplexität erzeugen?

---

# 4. Untersuchungsbereiche

## 4.1 Passwortsicherheit

Prüfung insbesondere von:

* Passwortlängen
* Passwortkomplexität
* Passwortänderungsprozessen
* Passwort-Hashing
* Passwort-Reset-Verfahren

Zu bewerten:

* aktueller Sicherheitsstand
* notwendige Verbesserungen
* Aufwand und Nutzen möglicher Maßnahmen

---

## 4.2 Session-Sicherheit

Prüfung insbesondere von:

* Session Lifecycle
* Session Invalidierung
* Session Timeout
* Session Fixation Schutz
* Session Cookies
* Logout-Verhalten

Zu bewerten:

* aktueller Schutzgrad
* notwendige Anpassungen

---

## 4.3 Benutzerverwaltung

Prüfung insbesondere von:

* Schutz des letzten ADMIN-Kontos
* Deaktivierung von Benutzern
* Rollenänderungen
* Passwortänderungen durch Administratoren

Zu bewerten:

* Risiken administrativer Fehlbedienung
* notwendige Schutzmechanismen

---

## 4.4 Recovery- und Notfallfähigkeit

Prüfung insbesondere von:

* Verlust des letzten Administratorkontos
* Passwortverlust
* Wiederherstellung administrativer Zugriffe
* Dokumentation von Recovery-Verfahren

Zu bewerten:

* technische und organisatorische Absicherung

---

## 4.5 Angriffsflächen

Prüfung insbesondere von:

* öffentliche Endpunkte
* API-Schutz
* Fehlerbehandlung
* Informationslecks
* Rollenprüfungen

Zu bewerten:

* realistisches Risikoprofil der Anwendung

---

# 5. Nicht Bestandteil dieser Anforderung

Nicht Ziel von BL-014 sind:

* Penetrationstests
* externe Security Audits
* Zertifizierungen
* Identity Provider (OAuth, Keycloak usw.)
* Multi-Faktor-Authentifizierung
* Enterprise-Security-Architekturen
* SIEM-Lösungen
* komplexe Compliance-Anforderungen

Die EMC Mitgliederverwaltung bleibt ein pragmatisches Vereinsprojekt.

---

# 6. Erwartete Ergebnisse

BL-014 soll liefern:

* Sicherheitsanalyse des Ist-Zustands
* Bewertung identifizierter Risiken
* Priorisierte Maßnahmenliste
* Architektur- und Governance-Empfehlungen
* Entscheidung über sinnvolle Umsetzungen
* Definition möglicher Folgeanforderungen

---

# 7. Erfolgskriterien

BL-014 gilt als erfolgreich abgeschlossen, wenn:

* die bestehenden Sicherheitsmechanismen bewertet wurden
* relevante Risiken identifiziert wurden
* sinnvolle Maßnahmen priorisiert wurden
* unnötige Komplexität bewusst ausgeschlossen wurde
* die Ergebnisse dokumentiert wurden
* erforderliche Folgeanforderungen definiert wurden

---

# 8. Erwartete Folgeergebnisse

Mögliche Ergebnisse können sein:

* direkte Umsetzungen innerhalb von BL-014
* neue Sicherheitsanforderungen
* neue Governance-Regeln
* Anpassungen der Betriebsdokumentation
* Anpassungen der Entwicklerdokumentation

Die konkrete Umsetzung wird auf Basis der Analyse entschieden.
