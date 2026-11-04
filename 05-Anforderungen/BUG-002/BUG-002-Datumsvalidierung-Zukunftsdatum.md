# BUG-002 – Fachliche Validierung zukünftiger Datumswerte

Status: Offen

---

# Fehlerbeschreibung

Im React-Frontend der EMC-Mitgliederverwaltung kann bei einem Mitglied ein **Geburtsdatum in der Zukunft** eingegeben und gespeichert werden.

Ein zukünftiges Geburtsdatum ist fachlich ungültig und muss abgewiesen werden.

Der Fehler wurde bei den Funktionstests zur Datenmigration **BL-027** in der lokalen Entwicklungsumgebung gegen die DEV-Datenbank festgestellt.

Zusätzliche Beobachtungen:

- Beim **Datenschutzdatum** ist bereits eine Prüfung auf zukünftige Werte vorhanden.
- Im Bereich **Chorkleidung** ist die Zukunftsprüfung teilweise vorhanden. Ob die bestehenden Regeln fachlich richtig sind, ist noch offen.
- Ein zukünftiges **Eintrittsdatum** kann fachlich sinnvoll sein; die konkrete Regel ist noch zu klären.
- Ein zukünftiges **Austrittsdatum** ist fachlich sinnvoll, etwa bei einem angekündigten Austritt zum Jahresende.

Diese Beobachtungen bedeuten nicht, dass alle genannten Felder fehlerhaft sind.

---

# Ursache

Noch nicht untersucht.

Zu prüfen ist insbesondere, ob die Validierung des Geburtsdatums fehlt, nicht aufgerufen wird oder beim Speichern umgangen werden kann.

---

# Fachlicher Prüfpunkt – alle Datumsfelder

**Sämtliche fachlichen Datumsfelder der EMC-Mitgliederverwaltung** sind systematisch darauf zu prüfen, ob ein Datum in der Zukunft zulässig ist.

Für jedes Feld ist festzuhalten:

- Fachliche Bedeutung des Datumsfeldes.
- Ob zukünftige Datumswerte **zulässig**, **unzulässig** oder **nur unter bestimmten Bedingungen zulässig** sind.
- Welche Validierung derzeit in Access, Frontend und gegebenenfalls Backend besteht.
- Ob die tatsächliche Validierung der festgelegten fachlichen Regel entspricht.
- Ob eine Anpassung erforderlich ist und in welchem System sie umgesetzt werden muss.

Die Prüfung umfasst auch Datumsfelder, die derzeit ausschließlich in Access oder direkt in Tabellen bearbeitet werden können, insbesondere die im Rahmen von BL-027 migrierten Felder.

**Bereits bekannte fachliche Einordnung:**

| Datumsfeld / Bereich | Vorläufige Einordnung |
|---|---|
| Geburtsdatum | Zukunft unzulässig; bestätigter Fehler |
| Eintrittsdatum | Zukunft grundsätzlich denkbar; fachliche Regel klären |
| Austrittsdatum | Zukunft zulässig, z. B. Austritt zum Jahresende |
| Datenschutzdatum | Zukunftsprüfung vorhanden; Regel und Umsetzung prüfen |
| Chorkleidung | Teilweise Zukunftsprüfung vorhanden; Fachlichkeit prüfen |
| Übrige fachliche Datumsfelder | Einzeln prüfen und bewerten |

Die fachliche Prüfung ist von der technischen Umstellung der Datenbankspalten von `DATETIME` auf `DATE` in BL-027 zu trennen.

---

# Umsetzung

Noch offen.

Nach der fachlichen Prüfung sind die notwendigen Korrekturen festzulegen. Für das Geburtsdatum ist sicherzustellen, dass ein zukünftiger Wert nicht gespeichert werden kann.

---

# Ergebnis

Noch offen.

---

# Dokumentation

Betroffene bzw. zu prüfende Komponenten:

- React-Frontend (Formular- und Datumsvalidierung)
- Java-/Spring-Boot-Backend (serverseitige Validierung)
- Access-Mitgliederverwaltung (fachliche Datumsfelder und Eingaberegeln)

Die DEV-Datenmigration aus BL-027 ist funktional geprüft; BUG-002 wird separat bearbeitet.

---

# Abschlussbewertung

BUG-002 ist erfasst, aber noch nicht behoben.

Status:

```text
BUG-002
OFFEN
```
