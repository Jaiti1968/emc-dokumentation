# BL-027 – Umsetzungsergebnis

**Datei:** `BL-027-umsetzung-ergebnis-v0.2.md`  
**Ablagepfad:** `05-Anforderungen/BL-027/BL-027-umsetzung-ergebnis-v0.2.md`  
**Version:** 0.2  
**Stand:** 04.11.2026  
**Status:** Technisch in DEV und PROD umgesetzt; abschließende Dokumentationspflege noch offen

## 1. Zweck und Abgrenzung

Dieses Dokument schreibt den Umsetzungsstand aus `BL-027-umsetzung-ergebnis-v0.1.md` fort. Bestandsaufnahme, Anforderungsdokument und isoliertes Migrationstestergebnis bleiben als Nachweise ihrer jeweiligen Bearbeitungsphase erhalten.

BL-027 harmonisiert fachliche Kalenderdaten als MariaDB `DATE`, Java `LocalDate` und API-Datum `YYYY-MM-DD`. Die fachlichen Berechnungsregeln werden nicht verändert. Drei bewusst ausgenommene technische beziehungsweise Legacy-`DATETIME`-Felder bleiben unverändert.

## 2. Backend

- Fachliche Datumsverarbeitung auf `LocalDate` harmonisiert; JDBC-Mapping für fachliche Datumsfelder angepasst, insbesondere `ResultSet.getDate(...).toLocalDate()`.
- Technische Zeitstempel nicht verändert.
- Releaseversion für PROD: `1.2.1`; vorherige DEV-Version: `1.2.1-SNAPSHOT`.
- `mvnw clean verify`: **99 Tests erfolgreich**.
- PROD-Image `emc-mitglieder-backend:1.2.1` erstellt; Alias `emc-mitglieder-backend:prod` auf dieses Image gesetzt und bereitgestellt.
- In PROD `/api/system/info` geprüft: Backendversion `1.2.1`, Umgebung `PROD`, Spring-Profil `prod`; Actuator-Healthstatus `UP`.

## 3. Frontend

- Datumsprüfung `isDateInFuture()` auf lokalen Kalendertag statt UTC-basierter ISO-Ableitung umgestellt.
- Fachliche Anzeige und bestehende Bedienlogik ansonsten beibehalten.
- Releaseversion für PROD: `1.2.1`; vorherige DEV-Version: `1.2.1-SNAPSHOT`.
- **65 Frontend-Unit-Tests erfolgreich**, Vite-Build erfolgreich.
- ARM64-Image `emc-mitglieder-frontend:1.2.1` erstellt; Alias `emc-mitglieder-frontend:prod` auf dieses Image gesetzt und bereitgestellt.
- PROD-Versionsanzeige von Frontend und Backend mit `1.2.1` geprüft.

## 4. Datenbankmigration

### 4.1 Isolierter Migrationstest

In `emc_mitglieder_migration_test` wurden die 14 fachlichen Spalten in einer vollständigen DEV-Datenkopie erfolgreich umgestellt. Datenvergleich und wesentliche Access-Tests waren erfolgreich. Einzelheiten stehen in `DATE-Migration-Migrationstest-Ergebnis-v0.1.md`.

### 4.2 TEST

Die Backend-Testdatenbank `emc_mitglieder_test` wurde am 29.10.2026 auf die DATE-Semantik harmonisiert. Die 14 betroffenen Spalten wurden als `DATE` und die drei ausgenommenen Felder weiterhin als `DATETIME` bestätigt. Eine zusätzliche DATE-Spalte in der temporären Tabelle `tblDatumKompatibilitaet` ist kein Bestandteil der 14 Migrationsfelder. Anschließend waren 99 Backend-Tests erfolgreich.

### 4.3 DEV

Am 02.11.2026 wurden in `emc_mitglieder_dev` alle 14 fachlichen Spalten in sieben Tabellen von `DATETIME` auf `DATE` migriert.

- Vorheriger SQL-Export: `emc_mitglieder_dev_pre_BL-027_2026-11-02.sql`.
- Automatisiertes Backup `202611020300.emc_mitglieder_dev.sql.gz` vorhanden; gzip-Integrität geprüft.
- Zeilenzahlen der betroffenen Tabellen vor und nach der Migration unverändert.
- Keine fachlich relevanten Uhrzeitanteile festgestellt.
- SQL-Export nach der Migration erstellt.
- Lokale Integrations- und Access-Kompatibilitätstests erfolgreich; anschließend NAS-DEV-Bereitstellung mit `1.2.1-SNAPSHOT` und Funktionstests durchgeführt.

### 4.4 PROD

Am 04.11.2026 wurde `emc_mitglieder_prod` kontrolliert migriert.

- Vorheriger SQL-Export: `/volume1/docker/backups/mariadb/emc_mitglieder_prod_pre_BL-027_20261104.sql` (ca. 97 KB); Sicherung archiviert.
- Alle **14 fachlichen Datumsfelder in sieben Tabellen** von `DATETIME` auf `DATE` umgestellt.
- Schema-Nachkontrolle über `information_schema` durchgeführt.
- Die bewusst ausgenommenen `DATETIME`-Felder blieben von BL-027 unberührt.
- Die bisherige Access-Anwendung wurde nach der PROD-Migration gesondert getestet und funktioniert weiterhin.
- Backend und Frontend `1.2.1` in PROD bereitgestellt und Versions-/Healthprüfungen erfolgreich durchgeführt.
- Automatisiertes tägliches MariaDB-Backup ist weiterhin funktionsfähig.

Die PROD-Migration und die Bereitstellung sind abgeschlossen. Das vorhandene SQL-Backup ist ein Sicherungsnachweis, aber kein dokumentierter praktischer Restore-Test.

## 5. Funktions- und Kompatibilitätsprüfungen

Die lokalen Tests gegen die migrierte DEV-Datenbank sind in v0.1 im Einzelnen dokumentiert. Erfolgreich geprüft wurden dort unter anderem die Mitgliederliste, die Pflege fachlicher Datumswerte, Access-Datenoperationen sowie Access-Berichte einschließlich Alters- und Zugehörigkeitsberechnung.

Nach den NAS-Deployments wurden die DEV-Anwendung und anschließend die PROD-Anwendung geprüft. Für PROD sind insbesondere die erfolgreiche Versions-/Healthprüfung und die eigenständige Bestätigung der Funktionsfähigkeit der bisherigen Access-Anwendung dokumentiert. Eine vollständige erneute Einzelprüfung aller 14 Felder in PROD ist aus dem vorliegenden Nachweisstand nicht gesondert belegt; die detaillierten Feldtests stammen aus der DEV-Phase.

Die fachlichen Regeln bleiben unverändert: Alter im Bezugsjahr = Bezugsjahr minus Geburtsjahr; Zugehörigkeit im Bezugsjahr = Bezugsjahr minus Eintrittsjahr.

## 6. Nebenbefunde und Abgrenzung

**BUG-002 – Fachliche Validierung zukünftiger Datumswerte:** Bei den lokalen Tests wurde festgestellt, dass ein Geburtsdatum in der Zukunft gespeichert werden kann. Die fachliche Prüfung zulässiger Zukunftsdaten ist nicht Teil der technischen DATE-Migration und bleibt separat offen.

**BUG-003 – Frontend-Browser-Cache:** Nach einem Frontend-Deployment konnte eine veraltete Anzeige beziehungsweise eine weiße Seite auftreten. Die Cache-Korrektur wird getrennt in `1.2.2-SNAPSHOT` in DEV beobachtet. Sie ist nicht Bestandteil von Release `1.2.1` und kein Abschlusskriterium der DATE-Migration.

## 7. Repository- und Deployment-Stand

- Backend und Frontend für Release `1.2.1` in den jeweiligen `master` übernommen und in PROD bereitgestellt.
- Frontend-Release-Commit: `6faaa61`.
- Backend-Release-Commit: `3d7221f`; aufgrund eines GitHub-Pushfehlers über Pull Request #1 in `master` integriert (Merge-Commit `cb1dd0b`).
- PROD-Backend-Image: `emc-mitglieder-backend:1.2.1`; vorheriges `1.2.0`-Image für möglichen Rollback vorgehalten.
- PROD-Frontend-Image: `emc-mitglieder-frontend:1.2.1`; vorheriges `1.2.0`-Image ebenfalls vorgehalten.
- Frontend-PROD-Compose auf Image-Alias `emc-mitglieder-frontend:prod` umgestellt.

Ein Image-Rollback ist nicht gleichbedeutend mit einem Datenbank-Restore. Vor einer Rückkehr zu älteren Anwendungsversionen muss deren Verträglichkeit mit dem geänderten Datenbankschema gesondert beurteilt werden.

## 8. Noch offene Dokumentations- und Bereinigungsarbeiten

1. Anforderungsdokument `BL-027-harmonisierung-fachlicher-datumsfelder.md` und `02.30-backlog.md` auf den tatsächlich abgeschlossenen DEV-/PROD-Stand aktualisieren.
2. Deployment- und Backup-/Recovery-Dokumentation mit dem tatsächlich verwendeten Vorgehen und den Auswirkungen der DATE-Migration abgleichen.
3. Über die Bereinigung der temporären Tabelle `tblDatumKompatibilitaet` in `emc_mitglieder_test` und die weitere Verwendung der isolierten Migrationstestdatenbank entscheiden. Diese Arbeiten sind keine Voraussetzung für den produktiven DATE-Betrieb.
4. Abschlussstatus von BL-027 nach Dokumentationsabgleich festlegen.

## 9. Abschlussbewertung

**Die technische Harmonisierung der 14 fachlichen Datumsfelder ist in TEST, DEV und PROD durchgeführt.** Die Backend-/Frontend-Anpassungen sind mit Release `1.2.1` produktiv. Die bisherige Access-Anwendung wurde nach der PROD-Migration erfolgreich geprüft.

Die technische Umsetzung ist damit abgeschlossen. Für einen formalen Abschluss gemäß Backlog-Governance stehen noch die Aktualisierung der zugehörigen Projekt- und Betriebsdokumentation sowie die Statuspflege im Backlog aus. BUG-002 und BUG-003 bleiben eigenständige Vorgänge.
