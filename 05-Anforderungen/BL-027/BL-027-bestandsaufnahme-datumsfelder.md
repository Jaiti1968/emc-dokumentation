# BL-027 – Bestandsaufnahme der fachlichen Datumsfelder

**Datei:** `BL-027-bestandsaufnahme-datumsfelder.md`  
**Ablagepfad:** `05-Anforderungen/BL-027/BL-027-bestandsaufnahme-datumsfelder.md`  
**Stand:** 29.10.2026  
**Status:** Abgeschlossen (Bestandsaufnahme)  
**Anforderung:** BL-027 – Harmonisierung der fachlichen Datumsfelder

---

# Inhaltsverzeichnis

1. Anlass und Ziel der Bestandsaufnahme
2. System- und Datenbankausgangslage
3. Inventar der fachlichen Datumsfelder
4. Bewusst ausgenommene Zeitstempelfelder
5. Datenqualität und Migrationsvoraussetzungen
6. Microsoft Access und ODBC
7. Backend – Ausgangsbefund
8. Frontend – Ausgangsbefund
9. Fachliche Berechnungsregeln
10. Bewertung und Handlungsbedarf
11. Abgrenzung, Referenzen und Folgeschritte

---

# 1. Anlass und Ziel der Bestandsaufnahme

Die EMC Mitgliederverwaltung verarbeitet fachliche Kalenderdaten in MariaDB, der weiterhin eingesetzten Microsoft-Access-Anwendung, einem Java-/Spring-Boot-Backend und einem React-/Vite-Frontend.

In der Datenbank waren diese Werte ursprünglich überwiegend als `DATETIME` definiert, obwohl fachlich ausschließlich ein Kalenderdatum benötigt wird. Ziel von BL-027 ist eine konsistente Verarbeitung als MariaDB `DATE`, Java `LocalDate` und JSON-Datum im Format `YYYY-MM-DD`; die Benutzeroberfläche zeigt Datumswerte im deutschen Format `DD.MM.YYYY`.

Dieses Dokument hält den **vor der isolierten Testmigration ermittelten Ausgangszustand** fest. Die Durchführung und Ergebnisse der Migration in der gesonderten Testdatenbank sind nicht Gegenstand dieser Bestandsaufnahme, sondern des eigenständigen Migrationstest-Ergebnisdokuments.

# 2. System- und Datenbankausgangslage

- Datenbankplattform: MariaDB auf der bestehenden NAS-Infrastruktur.
- Relevante Umgebungen: `emc_mitglieder_dev`, `emc_mitglieder_test`, `emc_mitglieder_prod`.
- Microsoft Access greift über den **MariaDB ODBC 3.2 Driver** auf verknüpfte Tabellen zu.
- Das Backend verwendet JDBC und stellt REST-Schnittstellen bereit; das Frontend verarbeitet die gelieferten Datumsstrings.

Bei der Bestandsaufnahme wurden in `emc_mitglieder_dev` insgesamt **17 Spalten des Typs `DATETIME`** und keine Spalten des Typs `DATE` oder `TIMESTAMP` festgestellt. Davon sind **14 fachliche Datumsfelder** und **3 bewusst unverändert zu belassende Felder**.

Die Datenbankstandards der Umgebungen unterscheiden sich teilweise in ihren Standard-Collations. Diese Unterschiede sind kein Gegenstand der Datentypmigration und sollen nicht beiläufig verändert werden.

# 3. Inventar der fachlichen Datumsfelder

Die folgenden **14 Spalten** wurden als Kandidaten für `DATETIME` → `DATE` identifiziert:

| Nr. | Tabelle | Spalte | Ausgangstyp | Zieltyp | Fachlicher Zweck |
|---:|---|---|---|---|---|
| 1 | `tblAllgemein_FT` | `Bezugsdatum` | `DATETIME NOT NULL` | `DATE NOT NULL` | Bezugsdatum des Geschäftsjahres |
| 2 | `tblChorkleidung` | `UebergabeAm` | `DATETIME NULL` | `DATE NULL` | Übergabe |
| 3 | `tblChorkleidung` | `DatumAnteil` | `DATETIME NULL` | `DATE NULL` | Anteil/Abrechnung |
| 4 | `tblChorkleidung` | `RueckgabeAm` | `DATETIME NULL` | `DATE NULL` | Rückgabe |
| 5 | `tblChorkleidung` | `Kaufdatum` | `DATETIME NULL` | `DATE NULL` | Kauf |
| 6 | `tblChorkleidung` | `SommerkleidungErhalten` | `DATETIME NULL` | `DATE NULL` | Sommerkleidung – Erhalt |
| 7 | `tblChorkleidung` | `SommerkleidungRueckgabe` | `DATETIME NULL` | `DATE NULL` | Sommerkleidung – Rückgabe |
| 8 | `tblDatenschutz` | `DatumDatenschutz` | `DATETIME NULL` | `DATE NULL` | Datenschutzdatum |
| 9 | `tblEhrungen` | `EhrungenTermin` | `DATETIME NULL` | `DATE NULL` | Ehrungstermin |
| 10 | `tblFunktionen` | `FunktionenTerminvon` | `DATETIME NULL` | `DATE NULL` | Beginn einer Funktion |
| 11 | `tblFunktionen` | `FunktionenTerminbis` | `DATETIME NULL` | `DATE NULL` | Ende einer Funktion |
| 12 | `tblMitglieder` | `Geburtsdatum` | `DATETIME NULL` | `DATE NULL` | Geburtstag |
| 13 | `tblMitgliedschaft` | `Eintritt` | `DATETIME NULL` | `DATE NULL` | Eintrittsdatum |
| 14 | `tblMitgliedschaft` | `Austritt` | `DATETIME NULL` | `DATE NULL` | Austrittsdatum |

Für die 13 optionalen Felder wurde `NULL` als zulässiger Wert und als Default festgestellt. `tblAllgemein_FT.Bezugsdatum` ist dagegen `NOT NULL` ohne Default. Für die 14 Felder waren keine zusätzlichen `EXTRA`-Attribute oder Spaltenkommentare hinterlegt.

# 4. Bewusst ausgenommene Zeitstempelfelder

| Tabelle | Spalte | Begründung |
|---|---|---|
| `tblAllgemein_FT` | `TerminkalenderDatumSelektiert` | Nicht genutztes Legacy-Feld des Kalenders; keine Umstellung im Rahmen von BL-027 vorgesehen |
| `tblUsers` | `created_at` | Technischer Erstellungszeitstempel; Uhrzeit ist fachlich/technisch relevant |
| `tblUsers` | `last_login_at` | Technischer Anmeldezeitstempel; Uhrzeit ist relevant |

`tblUsers.created_at` besitzt in der Ausgangsstruktur den Default `current_timestamp()`. Die technischen Zeitstempel bleiben `DATETIME`; eine pauschale Konvertierung aller Datums-/Zeitspalten wäre falsch.

# 5. Datenqualität und Migrationsvoraussetzungen

## 5.1 Uhrzeitanteile

Für alle 14 fachlichen Kandidaten wurde in DEV lesend geprüft, ob Datensätze mit einer Uhrzeit ungleich `00:00:00` vorhanden sind. **Ergebnis: keine solchen Datensätze.** Damit ist nach der Bestandsaufnahme kein fachlicher Informationsverlust durch das Entfernen des Uhrzeitanteils zu erwarten.

## 5.2 Bezugsdatum

`tblAllgemein_FT.Bezugsdatum` wird als **31. Dezember des Geschäftsjahres** geführt. Zum Untersuchungszeitpunkt war das Geschäftsjahr `2026` und das Bezugsdatum `2026-12-31 00:00:00`.

## 5.3 Testdatenbanken

Die bestehende Datenbank `emc_mitglieder_test` ist eine strukturell orientierte Testumgebung mit eigenständigem Datenbestand und wird für automatisierte Backend-Tests genutzt. Sie eignet sich nicht als unveränderte vollständige Kopie der DEV-Daten für einen isolierten Vergleich. Deshalb wurde für den späteren Migrationstest eine separate vollständige DEV-Kopie unter `emc_mitglieder_migration_test` vorgesehen.

**Wichtig:** Die eigentliche Umstellung von DEV, TEST und PROD ist von der Bestandsaufnahme zu trennen und jeweils kontrolliert durchzuführen.

# 6. Microsoft Access und ODBC

## 6.1 Ausgangslage

Die Access-Anwendung verwendet verknüpfte MariaDB-Tabellen über den **MariaDB ODBC 3.2 Driver**. Die Verträglichkeit des MariaDB-Typs `DATE` mit Access war vor einer produktiven Umstellung ausdrücklich zu prüfen.

## 6.2 Voruntersuchung

In der gesonderten Datenbank `emc_mitglieder_test` wurde vor dem vollständigen Migrationstest eine temporäre Tabelle `tblDatumKompatibilitaet` mit einer Spalte `Testdatum DATE NULL` angelegt. Über eine Access-Verknüpfung konnten ein Datum und `NULL` gelesen sowie das Datum in Access geändert und anschließend in MariaDB wieder ausgelesen werden.

Dies lieferte einen ersten positiven Kompatibilitätshinweis, ersetzte aber noch keine Tests mit den echten Fachformularen und Tabellen.

## 6.3 Abfragen und VBA

- **34 exportierte gespeicherte Access-SQL-Abfragen** wurden auf erkennbare Abhängigkeiten vom Datentyp `DATETIME` geprüft; es ergaben sich keine identifizierten Inkompatibilitäten.
- Der exportierte Access-VBA-Code (etwa 2.432 Zeilen) wurde auf die Datumsverarbeitung untersucht. Dabei wurden unter anderem dynamische Datums-Literale in SQL, DAO-Zuweisungen und Datumswerte in der Dokumentgenerierung berücksichtigt. Es wurde keine zwingende Abhängigkeit von einem Uhrzeitanteil der 14 Fachfelder festgestellt.
- Die Access-Verknüpfungen werden beim Programmstart zentral über `dbconfig.ini` und VBA (`BuildMariaDbConnectString`, `RefreshLinkedTablesFromConfig`) konfiguriert. Diese technische Besonderheit wurde im anschließenden Migrationstest praktisch bestätigt und ist für jede isolierte Access-Testumgebung zu berücksichtigen.

Die Ergebnisse der vollständigen Access-Funktionstests sind im Migrationstest-Ergebnis dokumentiert.

# 7. Backend – Ausgangsbefund

Das Java-/Spring-Boot-Backend verwendet für die fachlichen Datumsfelder bereits überwiegend **`LocalDate`**, insbesondere in DTOs und bei der Verarbeitung der Mitglieder-, Mitgliedschafts-, Datenschutz- und Chorkleidungsdaten.

Identifizierte technische Einzelheiten:

- `MemberMapper.java` prüft bei mindestens einer Datumszuordnung zunächst mit `ResultSet.getTimestamp(...)` auf einen vorhandenen Wert und liest anschließend über `ResultSet.getDate(...).toLocalDate()` das Datum. Dies sollte auf eine konsistente reine Datumsverarbeitung geprüft werden.
- Reporting-Repositories lesen Datumswerte bereits mit `getDate(...).toLocalDate()`.
- Im `MemberRepository` werden `LocalDate`-Werte über JDBC übergeben.
- Technische Benutzerzeitstempel verwenden weiterhin `LocalDateTime` und sollen unverändert bleiben.

**Bewertung:** Nach den bisherigen Codebefunden erscheint keine umfassende Neugestaltung der Backend-Datumsmodelle erforderlich. Der konkrete Anpassungsbedarf und die Tests sind im weiteren Verlauf von BL-027 am aktuellen Code zu verifizieren.

# 8. Frontend – Ausgangsbefund

Das React-/Vite-Frontend verarbeitet fachliche Datumswerte im API-Format `YYYY-MM-DD` und stellt sie für Anwender im deutschen Datumsformat dar.

Ein konkreter Prüfpunkt wurde in den Datumshelfern identifiziert: Die Ermittlung des heutigen Tages über

```javascript
new Date().toISOString().slice(0, 10)
```

verwendet das **UTC-Datum**. In Randzeiten kann dieses vom lokalen Kalendertag abweichen und damit beispielsweise eine Prüfung auf zukünftige Daten beeinflussen.

**Bewertung:** Eine lokalzeitgerechte Ermittlung des heutigen Kalendertages sowie entsprechende Grenzfalltests sind im Rahmen von BL-027 zu prüfen. Die vorhandenen Formularvalidierungen sollen fachlich unverändert bleiben, sofern kein konkreter Fehler vorliegt.

# 9. Fachliche Berechnungsregeln

Die Harmonisierung der Datentypen darf keine fachliche Berechnungsregel verändern.

## 9.1 Alter im Bezugsjahr

Das angezeigte Alter entspricht dem Alter, das ein Mitglied **im Bezugsjahr erreicht**, unabhängig davon, ob der Geburtstag im laufenden Jahr bereits stattgefunden hat.

**Formel:** `Bezugsjahr − Geburtsjahr`.

## 9.2 Mitgliedschaftsdauer

Die Mitgliedschaftsdauer ergibt sich aus **Bezugsjahr minus Eintrittsjahr**, unabhängig davon, ob der Eintrittsjahrestag schon erreicht wurde.

**Formel:** `Bezugsjahr − Eintrittsjahr`.

## 9.3 Bezugsjahr in Access und Backend

- Access verwendet das `Bezugsdatum` aus `tblAllgemein_FT` (31.12. des Geschäftsjahres).
- Das Backend verwendet das `Geschäftsjahr` aus `tblAllgemein_FT`.
- Die vorhandene Access-Berechnung mit `DateDiff("yyyy", ...)` und die jahresbasierte Berechnung im Backend entsprechen der fachlichen Zielregel und sollen nicht auf eine tagesgenaue Jubiläumsberechnung umgestellt werden.

# 10. Bewertung und Handlungsbedarf

| Thema | Ausgangsbewertung | Konsequenz |
|---|---|---|
| MariaDB-Fachfelder | 14 `DATETIME`-Spalten ohne benötigte Uhrzeit | Gezielte Umstellung auf `DATE` |
| Technische Zeitstempel | 3 `DATETIME`-Spalten außerhalb des Fachumfangs | Unverändert lassen |
| Datenqualität | Keine nicht-nullständigen Uhrzeitanteile in den 14 Kandidaten | Datenverlust durch Uhrzeitkürzung nach Befund nicht zu erwarten |
| Access | Grundsätzliche `DATE`-Kompatibilität im Vorversuch nachgewiesen | Vollständige Funktionstests vor DEV/PROD-Migration |
| Backend | Fachlich überwiegend bereits `LocalDate` | Kleine gezielte Bereinigung und Regressionstests prüfen |
| Frontend | Datumsstrings vorhanden, UTC-basierte Heute-Ermittlung auffällig | Lokalzeit-/Grenzfallprüfung und Tests |
| Fachliche Jahresberechnungen | Regel klar definiert und bereits implementiert | Unverändert erhalten |
| Umgebungen | DEV, TEST, PROD mit unterschiedlichen Zwecken | Getrennte Migration und kontrollierte Freigabe |

# 11. Abgrenzung, Referenzen und Folgeschritte

## 11.1 Abgrenzung

Diese Bestandsaufnahme ist **kein Nachweis**, dass die Datenbankänderung bereits in DEV oder PROD durchgeführt wurde. Sie dokumentiert den ermittelten Ausgangszustand und die daraus abgeleiteten Anforderungen. Die technische Umsetzung erfolgt erst in den nachfolgenden, separat geprüften Phasen.

Nicht Bestandteil sind eine Änderung der Datenbank-Collations, eine Umstellung technischer Zeitstempel, neue Fachfunktionen oder Änderungen an den Regeln für Alter und Zugehörigkeit.

## 11.2 Referenzen

- `BL-027-harmonisierung-fachlicher-datumsfelder.md` – freigegebenes Anforderungsdokument.
- `DATE-Migration-Migrationstest-Ergebnis-v0.1.md` – Ergebnis der isolierten Migration und der Access-Kompatibilitätstests.
- `EMC_Access_SQL_Abfragen.txt` – ausgewerteter Export der Access-Abfragen (Analysequelle).
- `EMC_Access_VBA_Export.txt` – ausgewerteter VBA-Export (Analysequelle).
- Backend- und Frontend-Repositories – technische Quellbasis für die anschließende Umsetzung.

## 11.3 Folgeschritte

1. Ergebnis des isolierten Migrationstests als Nachweis heranziehen.
2. Backend-Anpassungen am aktuellen Quellstand abschließend spezifizieren, umsetzen und testen.
3. Frontend-Datumsverarbeitung und Validierungen gezielt prüfen und testen.
4. Migration der DEV- und später der TEST-Datenbank kontrolliert vorbereiten und durchführen; Schema-Gleichstand sicherstellen.
5. Nach erfolgreicher Abnahme und dokumentierter Rückfallplanung PROD gesondert migrieren.
6. Testressourcen und Dokumentation abschließend bereinigen beziehungsweise konsolidieren.

---

# Ergebnis

Die Bestandsaufnahme identifiziert **14 fachliche `DATETIME`-Felder** als Kandidaten für die Umstellung auf `DATE`. Drei weitere `DATETIME`-Felder bleiben bewusst unverändert. Die Datenprüfung und die vorläufige Analyse von Access, Backend und Frontend zeigen keine grundsätzliche Hürde, machen jedoch eine kontrollierte, umgebungsweise Umsetzung mit Funktionstests erforderlich.

**Status der Bestandsaufnahme: Abgeschlossen.**
