# BL-027 – Umsetzungsergebnis (Zwischenstand)

**Datei:** `BL-027-umsetzung-ergebnis-v0.1.md`  
**Ablagepfad:** `05-Anforderungen/BL-027/BL-027-umsetzung-ergebnis-v0.1.md`  
**Version:** 0.1  
**Stand:** 02.11.2026  
**Status:** In Arbeit – lokale Integrationstests gegen migrierte DEV-Datenbank abgeschlossen; NAS-DEV-Deployment und PROD-Migration offen

## 1. Zweck und Abgrenzung

Dieses Dokument ergänzt das Anforderungsdokument `BL-027-harmonisierung-fachlicher-datumsfelder.md`, die Bestandsaufnahme und das Ergebnis des isolierten Migrationstests. Es dokumentiert den nachfolgenden Umsetzungs- und Prüfstand. Die älteren Dokumente bleiben als zeitlich abgegrenzte Nachweise bestehen.

BL-027 vereinheitlicht fachliche Kalenderdaten als MariaDB `DATE`, Java `LocalDate` und API-Datum `YYYY-MM-DD`, ohne fachliche Berechnungsregeln zu verändern. Die drei bewusst ausgenommenen technischen beziehungsweise Legacy-`DATETIME`-Felder bleiben unverändert.

## 2. Backend

- Fachliche Datumsverarbeitung auf `LocalDate` harmonisiert; JDBC-Mapping für fachliche Daten bereinigt, insbesondere Lesen über `ResultSet.getDate(...).toLocalDate()` statt unnötiger Timestamp-Zwischenschritte.
- Technische Zeitstempel bleiben von der Umstellung ausgenommen.
- Backend-Version: `1.2.1-SNAPSHOT`.
- 99 Backend-Tests erfolgreich; automatisierte Tests wurden mit der separaten Datenbank `emc_mitglieder_test` ausgeführt.
- Beim lokalen Start wurde zunächst eine veraltete Backend-Buildinformation (`1.2.0`) angezeigt. Nach `mvnw clean package -DskipTests` wurde die erwartete Version `1.2.1-SNAPSHOT` angezeigt. Ursache war ein veralteter Buildstand, kein Fehler der Datumsumstellung.

## 3. Frontend

- Die Prüfung `isDateInFuture()` ermittelt den heutigen Kalendertag lokal statt über eine UTC-basierte ISO-Konvertierung.
- Fachliche Datumsanzeige und bestehende Bedienlogik bleiben grundsätzlich unverändert.
- Frontend-Version: `1.2.1-SNAPSHOT`.
- 65 Frontend-Tests erfolgreich.

## 4. Migration der DEV-Datenbank

**Zieldatenbank:** `emc_mitglieder_dev`  
**Durchführung:** 02.11.2026

- Vor der Migration wurde ein SQL-Export `emc_mitglieder_dev_pre_BL-027_2026-11-02.sql` erstellt. Zusätzlich lag das automatisierte Backup `202611020300.emc_mitglieder_dev.sql.gz` unter `/volume1/docker/backups/mariadb/` vor; dessen gzip-Integrität wurde erfolgreich geprüft.
- Alle 14 im Anforderungsdokument festgelegten fachlichen Spalten wurden von `DATETIME` auf `DATE` umgestellt.
- Die drei ausgenommenen `DATETIME`-Felder blieben unverändert.
- Die Tabellenzeilenzahlen der sieben betroffenen Tabellen wurden vor und nach der Migration verglichen; keine Abweichungen festgestellt.
- Es wurden keine fachlich relevanten Uhrzeitanteile in den umzuwandelnden Werten festgestellt.
- Nach der Migration wurde zusätzlich ein SQL-Export des DEV-Datenstands erstellt.
- Die produktive Datenbank `emc_mitglieder_prod` wurde nicht verändert.

## 5. Lokale Funktions- und Kompatibilitätstests

Das lokal gestartete Backend und Frontend wurden gegen die bereits migrierte DEV-Datenbank geprüft.

| Prüfbereich | Ergebnis |
|---|---|
| Mitgliederliste und Suche im Frontend | Erfolgreich |
| Geburtsdatum – Anzeigen, Ändern, Speichern, Rücksetzen | Erfolgreich |
| Mitgliedschaft – Eintritt und Austritt | Erfolgreich |
| Datenschutzdatum | Erfolgreich |
| Chorkleidungsdaten | Erfolgreich |
| Access-DEV – Ehrungen und Funktionen: Anzeige, Änderung, Neuanlage, Löschen | Erfolgreich |
| Access – `tblAllgemein_FT.Bezugsdatum`: Änderung und Rücksetzung | Erfolgreich |
| Alle 14 migrierten fachlichen Datumsfelder einzeln geprüft | Erfolgreich, laut manueller Testdurchführung |
| Access-Berichte einschließlich Alters- und Zugehörigkeitsberechnung | Erfolgreich |

Die fachlichen Berechnungsregeln bleiben unverändert: Alter im Bezugsjahr = Bezugsjahr minus Geburtsjahr; Zugehörigkeit im Bezugsjahr = Bezugsjahr minus Eintrittsjahr.

## 6. Nebenbefunde und Abgrenzung

Bei der lokalen Prüfung wurde festgestellt, dass im Frontend ein Geburtsdatum in der Zukunft gespeichert werden kann. Der Fehler sowie die allgemeine fachliche Prüfung, für welche Datumsfelder zukünftige Werte zulässig sind, werden getrennt unter **BUG-002 – Datumsvalidierung Zukunftsdatum** behandelt. Die technische Typmigration wird dadurch nicht um fachliche Regeländerungen erweitert.

Ein zukünftiges Austrittsdatum wurde als zulässiger Anwendungsfall betrachtet. Für weitere Datumsfelder sind die fachlichen Regeln im Rahmen von BUG-002 zu klären.

### 6.1 Harmonisierung der Backend-Testdatenbank

Die separate Datenbank `emc_mitglieder_test` wurde am 29.10.2026 ebenfalls auf die fachliche DATE-Semantik umgestellt.

Die 14 vorgesehenen Datumsfelder besitzen den Datentyp `DATE`. Die drei bewusst ausgenommenen DATETIME-Felder blieben unverändert.

Die anschließende Strukturprüfung ergab 15 DATE-Felder und drei DATETIME-Felder. Die zusätzliche DATE-Spalte gehört zur temporären Tabelle `tblDatumKompatibilitaet`, die für frühere Access-/ODBC-Schreibtests angelegt wurde.

Nach der Schemaharmonisierung wurden 99 automatisierte Backend-Tests erfolgreich abgeschlossen.

Die Harmonisierung der TEST-Datenbank ist damit abgeschlossen. Die nicht mehr benötigte Tabelle `tblDatumKompatibilitaet` bleibt bis zur gesonderten Bereinigung bestehen.

## 7. Repository- und Deployment-Stand

- Backend- und Frontend-Änderungen befinden sich auf den jeweiligen Feature-Branches `feature/BL-027-datumsfelder` und wurden committed und gepusht.
- Noch kein Merge in `master`.
- Noch kein Deployment der neuen Backend-/Frontend-Version auf NAS-DEV.
- Noch keine Migration der produktiven Datenbank `emc_mitglieder_prod`.
- Die Schemaharmonisierung der separaten Backend-Testdatenbank `emc_mitglieder_test` wurde am 29.10.2026 abgeschlossen und am 02.11.2026 erneut durch eine Strukturabfrage bestätigt.

## 8. Offene Schritte

1. Ergebnisdokument und Phasenstand in der Projektdokumentation abgleichen.
2. Die temporäre Testtabelle `tblDatumKompatibilitaet` in `emc_mitglieder_test` nach abschließender Prüfung bereinigen.
3. Merge- und NAS-DEV-Deployment vorbereiten, durchführen und die bereitgestellte Anwendung gegen die migrierte DEV-Datenbank abnehmen.
4. Beobachtungsphase und Monitoring der DEV-Umgebung auswerten.
5. PROD-Migration mit aktuellem Backup, Vor-/Nachkontrollen und Rückfallplanung gesondert freigeben und durchführen; anschließend Access, Backend, Frontend und Berichte prüfen.
6. Über die weitere Verwendung der isolierten Migrationstestressourcen entscheiden; finale Dokumentation und Backlog aktualisieren.

## 9. Zwischenbewertung

**Die lokale BL-027-Integration gegen die migrierte DEV-Datenbank ist erfolgreich geprüft.** Auch die Schemaharmonisierung der separaten Backend-Testdatenbank ist abgeschlossen und bestätigt.

Die gesamte Anforderung bleibt **In Arbeit**, weil die NAS-DEV-Bereitstellung mit anschließender Funktionsprüfung und Beobachtungsphase sowie die PROD-Migration noch ausstehen.
