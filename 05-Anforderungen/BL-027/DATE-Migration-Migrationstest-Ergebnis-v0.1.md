# EMC Mitgliederverwaltung – DATE-Migration: Ergebnis des isolierten Migrationstests

**Version:** 0.1  
**Stand:** 29.10.2026  
**Status:** Migrationstest und wesentliche Access-Kompatibilitätstests erfolgreich; DEV-/TEST-/PROD-Migration noch nicht freigegeben

## 1. Ziel und Abgrenzung

Prüfung der Umstellung fachlicher Datumsfelder in MariaDB von `DATETIME` auf `DATE`, ohne fachliche Änderungen an Daten oder Berechnungen. Fachliche Datumswerte sollen künftig in der Datenbank `DATE`, im Java-Backend `LocalDate`, im JSON `YYYY-MM-DD` und im Frontend deutsch formatiert dargestellt werden. Technische Zeitstempel bleiben unverändert.

Die Tests erfolgten isoliert in **`emc_mitglieder_migration_test`**, einer vollständigen Kopie der DEV-Datenbank einschließlich Daten. Die regulären Datenbanken `emc_mitglieder_dev`, `emc_mitglieder_test` und `emc_mitglieder_prod` wurden im Rahmen der eigentlichen DATE-Migration nicht verändert. Ein versehentlicher Schreibzugriff über eine zunächst falsch konfigurierte Access-Testkopie auf DEV wurde erkannt und die Änderung laut Anwender wieder zurückgesetzt.

## 2. Datenbankmigration

### 2.1 Umgestellte Felder

| Tabelle | Feld | Neuer Typ | Bemerkung |
|---|---|---|---|
| `tblAllgemein_FT` | `Bezugsdatum` | `DATE NOT NULL` | Fachliches Bezugsdatum; 31.12. des Geschäftsjahres |
| `tblChorkleidung` | `UebergabeAm` | `DATE NULL` | |
| `tblChorkleidung` | `DatumAnteil` | `DATE NULL` | |
| `tblChorkleidung` | `RueckgabeAm` | `DATE NULL` | |
| `tblChorkleidung` | `Kaufdatum` | `DATE NULL` | |
| `tblChorkleidung` | `SommerkleidungErhalten` | `DATE NULL` | |
| `tblChorkleidung` | `SommerkleidungRueckgabe` | `DATE NULL` | |
| `tblDatenschutz` | `DatumDatenschutz` | `DATE NULL` | |
| `tblEhrungen` | `EhrungenTermin` | `DATE NULL` | |
| `tblFunktionen` | `FunktionenTerminvon` | `DATE NULL` | |
| `tblFunktionen` | `FunktionenTerminbis` | `DATE NULL` | |
| `tblMitglieder` | `Geburtsdatum` | `DATE NULL` | |
| `tblMitgliedschaft` | `Eintritt` | `DATE NULL` | |
| `tblMitgliedschaft` | `Austritt` | `DATE NULL` | |

**Bewusst nicht umgestellt:** `tblAllgemein_FT.TerminkalenderDatumSelektiert` (Legacy-Kalenderfeld), `tblUsers.created_at`, `tblUsers.last_login_at` (technische Zeitstempel). Diese drei Spalten verbleiben bei `DATETIME`.

### 2.2 Datenbasis und Validierung

- Migrationsdatenbank mit 16 Tabellen und insgesamt 1.384 Zeilen aus DEV angelegt.
- Vor der Umstellung: In den 14 fachlichen `DATETIME`-Spalten keine Werte mit Uhrzeit ungleich `00:00:00` festgestellt.
- Umstellung der 14 Spalten auf `DATE` erfolgreich durchgeführt.
- Nachkontrolle der Spaltentypen: **14 × `DATE`, 3 × `DATETIME`**; Nullbarkeit der betroffenen Felder beibehalten.
- Datenvergleiche zwischen DEV und Migrationstest für sämtliche 14 Felder: **keine Abweichungen**.
- `tblAllgemein_FT.Bezugsdatum` blieb auf `2026-12-31`, Geschäftsjahr 2026.
- Keine Änderungen an Zeichensätzen oder Kollationen im Zuge der DATE-Migration vorgenommen.

## 3. Microsoft-Access-Kompatibilität

### 3.1 Isolierte Testumgebung

- Separate Access-Testkopie `EMC_Vereinsverwalter_DATEmigration_TEST.accdb` im eigenen Ordner `migration_test`.
- Eigene `dbconfig.ini` mit Datenbank `emc_mitglieder_migration_test` und Benutzer `emc_access_migration_test_rw`.
- Eigener ODBC-System-DSN `NAS_EMC_MIG_TEST` mit MariaDB ODBC 3.2 Driver; Verbindungstest erfolgreich.
- Testbenutzer mit auf die Migrationsdatenbank begrenzten Rechten `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
- Die Access-Startlogik ruft `CheckMariaDBConnection()` und darin `RefreshLinkedTablesFromConfig()` auf. Letztere baut ODBC-Verknüpfungen bei jedem Start anhand von `BuildMariaDbConnectString()` und der **lokalen** `dbconfig.ini` neu auf. Deshalb waren anfängliche manuelle Umverknüpfungen nach einem Neustart wieder auf DEV zurückgesetzt worden.
- Nach Einrichtung des separaten Ordners mit eigener `dbconfig.ini`: korrekte Verknüpfungen und Anzeige der Umgebung `MIGRATION_TEST – V2.0` bestätigt.

**Sicherheits-/Betriebshinweis:** Die `dbconfig.ini` enthält das Datenbankpasswort im Klartext. Keine Passwörter in Ergebnisdokumente oder Screenshots übernehmen; Berechtigungen und sichere Ablage beachten. Eine im Verlauf versehentlich auf DEV geschriebene Geburtsdatumsänderung wurde laut Anwender zurückgesetzt.

### 3.2 Funktionale Tests

| Bereich | Durchführung | Ergebnis |
|---|---|---|
| `tblMitglieder.Geburtsdatum` | Anzeige, Änderung in Access, Prüfung in MariaDB, Rückänderung | Erfolgreich |
| `tblMitgliedschaft.Eintritt` | Anzeige, Änderung in Access, Prüfung in MariaDB, Rückänderung | Erfolgreich |
| `tblDatenschutz.DatumDatenschutz` | Änderung in Access, Prüfung in MariaDB | Erfolgreich; Rückänderung nicht ausdrücklich bestätigt |
| `tblChorkleidung` | Funktionstest, MariaDB-Kontrolle, Rücksetzung der Teständerungen laut Anwender | Erfolgreich; nicht jedes der sechs Felder einzeln protokolliert |
| `tblEhrungen.EhrungenTermin` | Änderung, Neuanlage und Löschung in Access; Datenbankkontrolle | Erfolgreich |
| `tblFunktionen.FunktionenTerminvon` / `FunktionenTerminbis` | Funktionstest laut Anwender | Erfolgreich; Einzeloperationen nicht separat protokolliert |
| Geburtstagsliste | Bericht geöffnet; Stichproben zum Alter im Bezugsjahr 2026 und zu runden Geburtstagen geprüft | Erfolgreich |
| Zugehörigkeitsbericht | Bericht geöffnet; Stichproben der Zugehörigkeitsjahre geprüft | Erfolgreich |

**Fachliche Berechnungsregeln bestätigt:**

- Alter im Bezugsjahr = `Bezugsjahr − Geburtsjahr`, unabhängig vom bereits erfolgten Geburtstag.
- Zugehörigkeitsjahre = `Bezugsjahr − Eintrittsjahr`, unabhängig vom Eintrittsjahrestag.
- Referenz ist das Geschäftsjahr 2026 mit Bezugsdatum 31.12.2026.

### 3.3 Grenzen der Prüfung

- `tblMitgliedschaft.Austritt` nicht ausdrücklich separat schreibend getestet.
- `tblAllgemein_FT.Bezugsdatum` über Berichte indirekt gelesen, aber nicht schreibend getestet (bewusstes Stammdatum).
- Keine vollständige Regression sämtlicher Access-Formulare, VBA-Abläufe und Berichte durchgeführt.
- Für Datenschutz ist die Rücksetzung des geänderten Testwerts nicht ausdrücklich bestätigt; vor Abschluss der Testumgebung noch prüfen.
- Für einzelne Chorkleidungsfelder fehlen getrennte Testprotokolle; dies stellt keinen beobachteten Fehler dar.

## 4. Gesamtbewertung

**Ergebnis: Erfolgreicher isolierter Datenbankmigrationstest und erfolgreiche wesentliche Access-Kompatibilitätsprüfung.** Die 14 fachlichen Datumsfelder konnten ohne festgestellte Datenabweichungen von `DATETIME` auf `DATE` umgestellt werden. Die geprüften Access-Lese-/Schreibfunktionen und jahresbezogenen Berichte zeigten keine Auffälligkeiten.

Dies ist **noch keine Freigabe für DEV oder PROD** und kein Nachweis einer vollständigen End-to-End-Kompatibilität mit Backend und Frontend.

## 5. Offene Schritte / empfohlene Reihenfolge

1. Verifizieren, dass sämtliche Testdatenänderungen in der Migrationsdatenbank zurückgesetzt beziehungsweise Testdatensätze entfernt sind, insbesondere `DatumDatenschutz`.
2. Backend prüfen und ggf. Datums-Mapping auf `LocalDate` vereinheitlichen; technische Zeitstempel (`LocalDateTime`) unverändert lassen.
3. Frontend prüfen, insbesondere lokale Datumsermittlung statt UTC-basiertem `toISOString().slice(0,10)` bei `isDateInFuture()`; vorhandene Tests ergänzen.
4. Backend-/Frontend-Tests und Integrationsprüfungen gegen eine `DATE`-kompatible Testdatenbank durchführen.
5. Reproduzierbares, gesichertes Migrationsskript mit Vor-/Nachkontrollen, Backup und Rollback-Konzept erstellen.
6. Schema der automatisierten Backend-Testdatenbank `emc_mitglieder_test` zu geeignetem Zeitpunkt angleichen; nicht unbeabsichtigt im isolierten Migrationstest verändern.
7. DEV-Migration erst nach gesonderter Freigabe durchführen, anschließend Anwendungstests und Beobachtungsphase.
8. PROD-Migration gesondert planen, sichern, freigeben und durchführen.
9. Temporäre Ressourcen (`emc_mitglieder_migration_test`, eigener DB-Benutzer, ODBC-DSN, Access-Testkopie) und ältere Kompatibilitätstestartefakte erst nach ausdrücklicher Entscheidung entfernen oder archivieren.

## 6. Dokumentationsstand

Dieses Dokument hält die im gemeinsamen Testverlauf berichteten Ergebnisse fest. Technische SQL-Protokolle, Datenbank-Dumps und vollständige automatisierte Testberichte sind **nicht** Bestandteil dieses Ergebnisdokuments. Die Aussagen zu manuell geprüften Access-Funktionen beruhen auf den vom Anwender durchgeführten und zurückgemeldeten Tests.
