# BL-027 – Harmonisierung der fachlichen Datumsfelder

**Datei:** `BL-027-harmonisierung-fachlicher-datumsfelder.md`

**Ablagepfad:** `05-Anforderungen/BL-027/BL-027-harmonisierung-fachlicher-datumsfelder.md`

**Stand:** 04.11.2026

**Status:** In Arbeit – technische Umsetzung abgeschlossen, Dokumentationsabschluss ausstehend

**Priorität:** Mittel

**Roadmap-Bezug:** P1 Technische Produktreife

---

# Inhaltsverzeichnis

1. Ausgangslage
2. Zielsetzung
3. Fachliche und technische Anforderungen
4. Datenintegrität und Kompatibilität
5. DEV-, TEST- und PROD-Umgebungen
6. Vorgehensweise und Bearbeitungsstand
7. Abgrenzung
8. Abnahmekriterien
9. Dokumentationsanforderungen
10. Abhängigkeiten und Referenzen

---

# 1. Ausgangslage

Vor BL-027 wurden fachliche Datumswerte in der MariaDB-Datenbank überwiegend als `DATETIME` gespeichert, obwohl fachlich ausschließlich ein Kalenderdatum ohne Uhrzeit benötigt wird. Das Java-/Spring-Boot-Backend verarbeitet diese Werte bereits überwiegend als `LocalDate`; das React-/Vite-Frontend stellt sie für die Benutzer in deutscher Datumsdarstellung dar. Zusätzlich greift eine bestehende Microsoft-Access-Anwendung über ODBC auf die Datenbank zu.

Die uneinheitliche Typisierung erschwert die eindeutige Behandlung fachlicher Datumswerte und birgt Risiken bei Konvertierung, Validierung und Zeitzonenbehandlung.

Zur Risikominimierung wurde vor der eigentlichen Umsetzung eine isolierte Datenbank `emc_mitglieder_migration_test` als vollständige Kopie der DEV-Daten angelegt. Dort wurden 14 fachliche Felder von `DATETIME` auf `DATE` umgestellt. Datenvergleiche ergaben keine Abweichungen. Die wesentlichen Access-Funktionen einschließlich Schreiboperationen und jahresbezogener Berichte wurden erfolgreich geprüft. Die regulären Datenbanken DEV, TEST und PROD wurden durch diesen **isolierten** Migrationstest nicht verändert; ihre spätere Umstellung erfolgte in eigenen Schritten.

Die Erkenntnisse sind im Ergebnisdokument `DATE-Migration-Migrationstest-Ergebnis-v0.1.md` festgehalten.

---

# 2. Zielsetzung

BL-027 harmonisiert die Verarbeitung **fachlicher Kalenderdaten** über Datenbank, Backend, Frontend und die weiterhin verwendete Access-Anwendung hinweg.

Zielzustand:

- **MariaDB:** fachliche Datumsfelder mit Typ `DATE`.
- **Java/Spring Boot:** fachliche Datumswerte als `LocalDate`.
- **API:** Datumswerte im ISO-Format `YYYY-MM-DD`, ohne Uhrzeit und Zeitzonenversatz.
- **Frontend:** korrekte Eingabe, Validierung und benutzerseitige Darstellung im deutschen Datumsformat `DD.MM.YYYY`.
- **Microsoft Access:** unveränderte fachliche Nutzbarkeit über ODBC.
- **Fachliche Logik:** keine Änderung der bestehenden Berechnungs- und Geschäftsregeln.

Die Umsetzung ist eine technische Harmonisierung und **keine funktionale Erweiterung**.

---

# 3. Fachliche und technische Anforderungen

## 3.1 Umfang der Datenbankumstellung

Die folgenden 14 Felder sind von `DATETIME` auf `DATE` umzustellen:

| Tabelle             | Feld                      |
| ------------------- | ------------------------- |
| `tblAllgemein_FT`   | `Bezugsdatum`             |
| `tblChorkleidung`   | `UebergabeAm`             |
| `tblChorkleidung`   | `DatumAnteil`             |
| `tblChorkleidung`   | `RueckgabeAm`             |
| `tblChorkleidung`   | `Kaufdatum`               |
| `tblChorkleidung`   | `SommerkleidungErhalten`  |
| `tblChorkleidung`   | `SommerkleidungRueckgabe` |
| `tblDatenschutz`    | `DatumDatenschutz`        |
| `tblEhrungen`       | `EhrungenTermin`          |
| `tblFunktionen`     | `FunktionenTerminvon`     |
| `tblFunktionen`     | `FunktionenTerminbis`     |
| `tblMitglieder`     | `Geburtsdatum`            |
| `tblMitgliedschaft` | `Eintritt`                |
| `tblMitgliedschaft` | `Austritt`                |

Die bestehende NULL-Zulässigkeit und sonstige Spaltendefinitionen müssen erhalten bleiben. Insbesondere bleibt `tblAllgemein_FT.Bezugsdatum` `NOT NULL`; die übrigen 13 Felder bleiben nullable. Vor der jeweiligen Migration ist sicherzustellen, dass keine fachlich relevanten Uhrzeitanteile verloren gehen.

## 3.2 Unveränderte technische Zeitstempel

Die folgenden `DATETIME`-Felder sind **nicht** Bestandteil der Umstellung:

- `tblAllgemein_FT.TerminkalenderDatumSelektiert` (bestehendes Legacy-Feld)
- `tblUsers.created_at`
- `tblUsers.last_login_at`

Weitere technische Zeitstempel bleiben ebenfalls in ihrer bisherigen fachlich erforderlichen Genauigkeit erhalten.

## 3.3 Backend

- Fachliche Datumsfelder sind durchgängig als `LocalDate` zu behandeln.
- JDBC-Lese- und Schreibzugriffe sowie Mapper sind auf die `DATE`-Semantik zu prüfen.
- Unnötige `Timestamp`-Zwischenschritte bei reinen Kalenderdaten sollen bereinigt werden, sofern erforderlich und durch Tests abgesichert.
- Bestehende API-Verträge dürfen nicht unbeabsichtigt verändert werden.
- Technische Zeitstempel und ihre Verarbeitung bleiben unverändert.
- Bestehende automatisierte Tests sind anzupassen oder gezielt zu ergänzen.

## 3.4 Frontend

- Fachliche Datumswerte sind ohne ungewollte Zeitzonenverschiebung zu verarbeiten.
- Eingaben, Darstellung und Validierung sind auf konsistente Kalenderdatums-Semantik zu prüfen.
- Insbesondere ist die Ermittlung des lokalen heutigen Datums zu prüfen; UTC-basierte Ableitungen können an Tagesgrenzen abweichen.
- Die bestehende deutschsprachige Oberfläche und das bisherige Bedienverhalten bleiben erhalten.
- Vorhandene Frontend-Tests sind gezielt zu ergänzen.

## 3.5 Fachliche Berechnungen

Die bestehenden Regeln bleiben unverändert:

- **Alter im Bezugsjahr:** `Bezugsjahr − Geburtsjahr`, unabhängig davon, ob der Geburtstag bereits stattgefunden hat.
- **Mitgliedschaftsdauer im Bezugsjahr:** `Bezugsjahr − Eintrittsjahr`, unabhängig vom Jahrestag.
- **Bezugsdatum in Access:** 31.12. des Geschäftsjahres; im Backend ist das festgelegte Geschäftsjahr maßgeblich.

Bestehende Auswertungen, insbesondere Geburtstagsliste und Zugehörigkeitsbericht, müssen dieselben Ergebnisse liefern wie zuvor.

---

# 4. Datenintegrität und Kompatibilität

## 4.1 Datenintegrität

- Vor jedem produktionsnahen Datenbankeingriff ist eine geeignete Sicherung einschließlich Wiederherstellungsmöglichkeit vorzusehen.
- Die Migration muss hinsichtlich Tabellen, Feldern, NULL-Werten und Datumsinhalten überprüfbar sein.
- Datumswerte dürfen weder verschoben noch abgeschnitten werden; ein zuvor vorhandener Uhrzeitanteil ist vor der Umstellung zu prüfen.
- Die Migration ist nachvollziehbar und wiederholbar zu dokumentieren.

## 4.2 Access-Kompatibilität

Die Access-Anwendung bleibt Bestandteil der Gesamtlösung. Zu gewährleisten sind:

- Lesen und Schreiben der umgestellten `DATE`-Felder über MariaDB ODBC.
- Funktion der betroffenen Formulare und Datenoperationen.
- Unveränderte Ergebnisse der jahresbezogenen Auswertungen.
- Korrekte Trennung der Datenbankverbindungen der jeweiligen Umgebung.

Die Access-Verbindung wird beim Start über `dbconfig.ini` und VBA neu aufgebaut. Deshalb sind getrennte Konfigurationsdateien für Test und regulären Betrieb erforderlich; eine bloße manuelle Änderung im Tabellenverknüpfungs-Manager reicht nicht aus.

---

# 5. DEV-, TEST- und PROD-Umgebungen

Die Umgebungen wurden getrennt und kontrolliert umgestellt:

- **`emc_mitglieder_migration_test`:** isolierte Kopie von DEV für den ersten Migrationstest; nach erfolgreichem Abschluss am 04.11.2026 gelöscht.
- **`emc_mitglieder_test`:** eigenständige Datenbank für automatisierte Backend-Tests; am 29.10.2026 harmonisiert und geprüft. Die temporäre Tabelle `tblDatumKompatibilitaet` ist noch auf ihre Entbehrlichkeit zu prüfen.
- **DEV:** 14 Datumsfelder am 02.11.2026 migriert; Integrationstests und NAS-Bereitstellung von `1.2.1-SNAPSHOT` erfolgreich durchgeführt.
- **PROD:** 14 Datumsfelder am 04.11.2026 nach gesonderter Sicherung migriert; Backend und Frontend `1.2.1` bereitgestellt. Access-Kompatibilität, Versionsinformationen und Healthchecks erfolgreich geprüft.

Die Umstellung in PROD erfolgte erst nach den vorangegangenen Prüfungen. Der isolierte Test allein war keine Freigabe für DEV oder PROD.

---

# 6. Vorgehensweise und Bearbeitungsstand

BL-027 bleibt eine übergreifende Backlog-Anforderung mit internen Phasen. Die technische Umsetzung ist abgeschlossen. Der formale Abschluss im Backlog erfolgt nach der noch ausstehenden Dokumentations- und Bereinigungsprüfung.

| Phase | Inhalt | Stand am 04.11.2026 |
| --- | --- | --- |
| 1 | Bestandsaufnahme der Datumsfelder und fachlichen Regeln | Abgeschlossen |
| 2 | Isolierter Migrationstest und Access-Kompatibilitätsprüfung | Abgeschlossen |
| 3 | Backend-Analyse, Anpassungen und Tests | Abgeschlossen – 99 Backend-Tests erfolgreich |
| 4 | Frontend-Analyse, Anpassungen und Tests | Abgeschlossen – 65 Frontend-Tests erfolgreich |
| 5 | Migration und Integrationstests in DEV sowie Harmonisierung der TEST-Datenbank | Abgeschlossen – einschließlich NAS-DEV-Bereitstellung |
| 6 | Kontrollierte PROD-Migration und Funktionsprüfung | Abgeschlossen am 04.11.2026 – Release `1.2.1` produktiv |
| 7 | Abschlussdokumentation und Bereinigung der Testressourcen | In Arbeit – Ergebnisdokument v0.3, teilweiser Rückbau erfolgt |

## 6.1 Abgeschlossene Arbeiten

**Datenbank und Access**

- Alle 14 festgelegten fachlichen Datumsfelder wurden in TEST, DEV und PROD auf `DATE` harmonisiert.
- Datumsinhalte, Schema und NULL-Zulässigkeit wurden im Rahmen der Migration geprüft; die drei bewusst ausgenommenen technischen beziehungsweise Legacy-`DATETIME`-Felder blieben unverändert.
- Die bisherigen Access-Funktionen einschließlich der jahresbezogenen Berichte wurden im Rahmen der DEV-Prüfung getestet. Die reguläre Access-Anwendung wurde nach der PROD-Migration gesondert erfolgreich geprüft.
- Die fachlichen Berechnungsregeln wurden nicht verändert.
- Vor der PROD-Migration wurde der SQL-Export `/volume1/docker/backups/mariadb/emc_mitglieder_prod_pre_BL-027_20261104.sql` erstellt und archiviert. Ein praktischer Restore-Test dieses Exports ist nicht gesondert dokumentiert.

**Backend und Frontend**

- Die erforderlichen Datumsanpassungen wurden umgesetzt und getestet; fachliche Kalenderdaten werden im Backend als `LocalDate` verarbeitet.
- Die Frontend-Datumsverarbeitung wurde geprüft und die Ermittlung des lokalen heutigen Datums korrigiert.
- 99 Backend-Tests und 65 Frontend-Tests erfolgreich; Builds erfolgreich.
- NAS-DEV mit `1.2.1-SNAPSHOT` geprüft und Backend/Frontend `1.2.1` in PROD bereitgestellt.
- Versionsinformationen und Backend-Healthstatus in PROD erfolgreich geprüft.

**Rückbau der isolierten Testumgebung**

- Die temporäre MariaDB-Datenbank `emc_mitglieder_migration_test` wurde am 04.11.2026 gelöscht.
- Die Windows-System-DSN `NAS_EMC_MIG_TEST` wurde entfernt.
- Zwei überflüssige Access-Testkopien im regulären DEV-Verzeichnis wurden gelöscht.
- Das lokale Verzeichnis `D:\Chor\Access\emc_mitglieder\migration_test__Löschkandidat` bleibt mit einer Access-Testversion, `dbconfig.ini` und zwei Analyseexporten zunächst erhalten.

## 6.2 Noch ausstehende Arbeiten

- Das Ergebnisdokument `BL-027-umsetzung-ergebnis-v0.3.md` in der Projektdokumentation als aktuellen Stand führen und den Backlog nach Abschluss der Dokumentationsprüfung aktualisieren.
- Deployment- und Backup-/Recovery-Dokumentation mit dem tatsächlich verwendeten Vorgehen abgleichen.
- Über die temporäre Tabelle `tblDatumKompatibilitaet` in `emc_mitglieder_test` entscheiden.
- Die nur im lokalen Migrationsverzeichnis vorhandenen Access-Analyseexporte dauerhaft archivieren; anschließend über die verbliebenen lokalen Testdateien entscheiden.

## 6.3 Abgrenzung und offene Fehler

Bei den Funktionstests wurde festgestellt, dass zukünftige Geburtsdaten akzeptiert werden können. Dieser Sachverhalt und die systematische fachliche Prüfung zukünftiger Datumswerte werden unter **BUG-002** gesondert bearbeitet und sind kein Teil der technischen Datentypmigration.

**BUG-003** betrifft die Frontend-Cache-Steuerung und wird separat in DEV mit `1.2.2-SNAPSHOT` beobachtet. Die Korrektur ist nicht Bestandteil des Releases `1.2.1` und kein Abschlusskriterium von BL-027.

## 6.4 Dokumentationsstand

Bestandsaufnahme und isolierter Migrationstest bleiben als eigenständige historische Nachweise bestehen. Der aktuelle technische Umsetzungsstand einschließlich PROD-Migration und Rückbau ist in `BL-027-umsetzung-ergebnis-v0.3.md` festgehalten.

BL-027 bleibt bis zum Dokumentationsabgleich formal **In Arbeit**; die technische Umsetzung in TEST, DEV und PROD ist abgeschlossen.

---

# 7. Abgrenzung

Nicht Bestandteil von BL-027 sind:

- fachliche Änderungen der Alters- oder Zugehörigkeitsberechnung
- neue Berichte oder zusätzliche Funktionen der Mitgliederverwaltung
- grundlegende Umgestaltung der Access-Anwendung
- Migration auf ein anderes Datenbanksystem
- pauschale Umstellung technischer Zeitstempel auf `DATE`
- Änderungen an Berechtigungsmodell, Authentifizierung oder Deployment-Architektur ohne unmittelbaren Bezug zur Datumsverarbeitung
- allgemeine Refactorings außerhalb des erforderlichen Migrationsumfangs

---

# 8. Abnahmekriterien

BL-027 gilt als abgeschlossen, wenn:

- die 14 festgelegten Felder in den vorgesehenen Datenbanken korrekt als `DATE` vorliegen
- NULL-Zulässigkeit und Datumsinhalte nachweislich erhalten sind
- die ausgenommenen technischen `DATETIME`-Felder unverändert geblieben sind
- Backend und API fachliche Datumswerte konsistent als `LocalDate` beziehungsweise `YYYY-MM-DD` verarbeiten
- das Frontend Datumswerte korrekt anzeigt, validiert und ohne Zeitzonenverschiebung verarbeitet
- die bestehenden fachlichen Berechnungen unverändert korrekte Ergebnisse liefern
- Access die betroffenen Daten weiterhin lesen und schreiben kann
- die relevanten automatisierten Tests und manuellen Integrationstests erfolgreich sind
- DEV und die Backend-TEST-Datenbank konsistent umgestellt und geprüft wurden
- PROD nach gesonderter Freigabe erfolgreich migriert und geprüft wurde
- Sicherungs-, Wiederherstellungs- und Prüfschritte dokumentiert sind
- Projektdokumentation und betroffene technische Dokumentation aktualisiert wurden
- über die weitere Verwendung oder Bereinigung der isolierten Migrationstestressourcen entschieden wurde

---

# 9. Dokumentationsanforderungen

In `emc-dokumentation` werden Anforderung, Phasenstatus, technische Entscheidungen, Testergebnisse und Abschluss festgehalten. Änderungen am Backend und Frontend werden in den jeweiligen Repositories nachvollziehbar dokumentiert. Datenbank- und Betriebsverfahren werden, soweit betroffen, mit der maßgeblichen technischen Betriebsdokumentation abgestimmt.

Insbesondere sind die Migrationsskripte, Vorher-/Nachher-Prüfungen, Access-Testergebnisse, DEV-/TEST-/PROD-Abnahmen und ein geeignetes Rückfallverfahren nachvollziehbar zu dokumentieren.

---

# 10. Abhängigkeiten und Referenzen

**Anforderung:**

- BL-027 – Harmonisierung der fachlichen Datumsfelder

**Vorliegende Arbeitsergebnisse:**

- `DATE-Migration-Migrationstest-Ergebnis-v0.1.md` – freigegebener isolierter Migrationstest und Access-Kompatibilitätsprüfung
- `BL-027-umsetzung-ergebnis-v0.3.md` – technisches Umsetzungsergebnis einschließlich DEV/PROD und Rückbau
- Bisherige Bestandsaufnahme der MariaDB-Datumsfelder und Backend-/Frontend-Datumsverarbeitung

**Technische Grundlagen:**

- MariaDB-Datenbanken `emc_mitglieder_dev`, `emc_mitglieder_test`, `emc_mitglieder_prod`
- Ehemalige isolierte Datenbank `emc_mitglieder_migration_test` (am 04.11.2026 gelöscht)
- Backend- und Frontend-Repositories
- Microsoft Access mit MariaDB ODBC 3.2 und umgebungsbezogener `dbconfig.ini`
- EMC-INFRA und bestehende Sicherungs-/Deployment-Verfahren

---

# Ergebnis

Die 14 fachlichen Datumsfelder wurden in TEST, DEV und PROD auf `DATE` umgestellt. Die erforderlichen Backend- und Frontend-Anpassungen wurden mit Release `1.2.1` produktiv bereitgestellt. Die reguläre Access-Anwendung ist nach der PROD-Migration weiterhin funktionsfähig.

Die isolierte MariaDB-Migrationstestdatenbank und ihre ODBC-System-DSN wurden entfernt; die lokale Archivbereinigung ist noch offen. BL-027 ist **technisch umgesetzt**, bleibt bis zum Abschluss der Dokumentationspflege und Backlog-Aktualisierung formal **In Arbeit**. Die unabhängig erfassten BUG-002 und BUG-003 werden separat behandelt.
