# BL-025 Passwort-vergessen-Prozess

## Status

Offen

## Roadmap-Bezug

P2 Fachliche Erweiterungen

## Priorität

Niedrig

---

# Zielsetzung

Ziel dieser Anforderung ist die Einführung eines einfachen und wartungsarmen Passwort-vergessen-Prozesses.

Die Lösung soll den Anforderungen der EMC Mitgliederverwaltung als kleine Vereinsanwendung entsprechen und ohne zusätzliche Infrastruktur auskommen.

Insbesondere soll kein automatisierter Mailversand und kein komplexes Self-Service-Verfahren eingeführt werden.

---

# Ausgangssituation

Aktuell können Passwörter ausschließlich durch einen Administrator geändert werden.

Verliert ein Benutzer sein Passwort, erfolgt die Kommunikation außerhalb der Anwendung und der Administrator vergibt manuell ein neues Passwort.

Dieser Prozess funktioniert grundsätzlich, ist jedoch in der Anwendung nicht sichtbar und wird nicht unterstützt.

---

# Motivation

Die Anwendung soll Benutzern ermöglichen, einen Passwortverlust direkt innerhalb der Anwendung zu melden.

Gleichzeitig soll die Lösung:

- einfach verständlich sein
- keinen Mailserver benötigen
- keine zusätzlichen Sicherheitsrisiken erzeugen
- mit dem bestehenden Rollenmodell kompatibel bleiben

---

# Fachliche Anforderungen

## FA-001 Passwort vergessen Funktion

Im Login-Bereich soll eine Funktion bereitgestellt werden:

```text
Passwort vergessen
```

Der Benutzer muss die Möglichkeit erhalten, eine Passwort-Anfrage auszulösen.

---

## FA-002 Kennzeichnung am Benutzerkonto

Nach Auslösung der Funktion wird am Benutzerkonto ein Kennzeichen gespeichert.

Beispiel:

```text
Passwort vergessen = Ja
```

Das Kennzeichen bleibt bestehen, bis die Anfrage bearbeitet wurde.

---

## FA-003 Hinweis für Administratoren

Administratoren sollen beim Login oder innerhalb der Benutzerverwaltung erkennen können, dass offene Passwort-Anfragen vorliegen.

Beispiel:

```text
Es liegen offene Passwort-Anfragen vor.
```

Die konkrete Darstellung kann im Rahmen der Umsetzung festgelegt werden.

---

## FA-004 Bearbeitung durch Administrator

Ein Administrator kann für den betroffenen Benutzer ein neues Passwort vergeben.

Der bestehende Prozess zur Passwortänderung bleibt erhalten.

---

## FA-005 Rücksetzen der Kennzeichnung

Nach erfolgreicher Passwortänderung wird das Kennzeichen automatisch entfernt.

Dadurch gilt die Anfrage als erledigt.

---

# Technische Anforderungen

## TA-001 Erweiterung Benutzerdatenmodell

Das Benutzerkonto benötigt eine Kennzeichnung für offene Passwort-Anfragen.

Beispiel:

```text
password_reset_requested
```

Die konkrete technische Umsetzung kann über:

- Datenbankspalte
- Statusfeld
- vergleichbare Lösung

erfolgen.

---

## TA-002 Frontend Integration

Die Login-Seite wird um die Funktion:

```text
Passwort vergessen
```

erweitert.

---

## TA-003 Administrationsoberfläche

Die Benutzerverwaltung soll offene Passwort-Anfragen sichtbar machen.

Mögliche Varianten:

- Hinweisdialog nach Login
- Kennzeichnung in der Benutzerliste
- separater Hinweisbereich

Die konkrete Umsetzung wird während der Realisierung festgelegt.

---

## TA-004 Rollenmodell

Die Bearbeitung von Passwort-Anfragen bleibt ausschließlich Administratoren vorbehalten.

Das bestehende Rollenmodell wird nicht erweitert.

---

# Abgrenzung

Nicht Bestandteil dieser Anforderung:

- E-Mail Versand
- automatischer Passwortversand
- Self-Service Passwortänderung
- Einmal-Links
- Token-basierte Passwort-Resets
- externe Identity Provider
- OAuth / OpenID Connect

Diese Themen können bei einer späteren öffentlichen Bereitstellung erneut bewertet werden.

---

# Abhängigkeiten

Mögliche zukünftige Verknüpfungen:

- BL-018 Mailversand
- zukünftige Sicherheits- und Benutzerverwaltungsanforderungen

Aktuell bestehen keine technischen Abhängigkeiten.

---

# Nutzen

Die Lösung bietet:

- einfache Bedienung für Benutzer
- transparente Bearbeitung für Administratoren
- keine zusätzliche Infrastruktur
- geringe technische Komplexität
- vollständige Kompatibilität mit dem aktuellen Betriebsmodell

---

# Akzeptanzkriterien

## AK-001

Ein Benutzer kann eine Passwort-Anfrage auslösen.

## AK-002

Die Anfrage wird am Benutzerkonto gespeichert.

## AK-003

Ein Administrator erkennt offene Passwort-Anfragen.

## AK-004

Ein Administrator kann ein neues Passwort vergeben.

## AK-005

Nach erfolgreicher Passwortänderung wird die Anfrage automatisch als erledigt markiert.

## AK-006

Es wird kein Mailserver benötigt.

## AK-007

Das bestehende Rollen- und Sicherheitsmodell bleibt unverändert.
