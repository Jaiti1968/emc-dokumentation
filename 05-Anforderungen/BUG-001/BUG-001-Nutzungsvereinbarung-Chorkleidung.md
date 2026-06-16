# BUG-001 – Nutzungsvereinbarung Chorkleidung

Status: Abgeschlossen

---

# Fehlerbeschreibung

In Access kann eine Nutzungsvereinbarung für Chorkleidung erzeugt werden.

Die zugrunde liegende Word-Vorlage `Nutzungsvereinbarung_Chorkleidung.dotm` wurde jedoch über einen fest im VBA-Code hinterlegten absoluten Pfad referenziert.

Dadurch funktionierte die Dokumenterzeugung nur auf dem Rechner, auf dem die Vorlage ursprünglich abgelegt wurde.

Bei Verwendung der Access-Arbeitskopie auf anderen Rechnern konnte die Vorlage nicht gefunden werden.

---

# Ursache

Die Word-Vorlage war nicht Bestandteil der gemeinsam verteilten Access-Arbeitskopie.

Zusätzlich wurde im VBA-Code ein fest verdrahteter Dateipfad verwendet.

Dadurch bestand eine unnötige Abhängigkeit von einer bestimmten lokalen Verzeichnisstruktur.

---

# Umsetzung

Die Vorlage `Nutzungsvereinbarung_Chorkleidung.dotm` wurde jeweils in das Quellverezichnis \\NAS-DH2300-C64C\apps\access\emc_mitglieder\dev bzw. \\NAS-DH2300-C64C\apps\access\emc_mitglieder\prod kopiert.

Damit diese Datei dann auch als lokale Arbeitskopie mit übertragen wird, wurde jeweils in der `Start_EMC_Vereinsverwalter_dev.` und `Start_EMC_Vereinsverwalter_prod.` entsprechend ein zusätzlicher Copy-Zweig aufgenommen.

```text
set WORD_SOURCE=\\NAS-DH2300-C64C\apps\access\emc_mitglieder\dev\Nutzungsvereinbarung_Chorkleidung.dotm

set WORD_TARGET=%TARGET_DIR%\Nutzungsvereinbarung_Chorkleidung.dotm

copy "%WORD_SOURCE%" "%WORD_TARGET%" /Y
if errorlevel 1 (
    echo Fehler: Die Nutzungsvereinbarung_Chorkleidung.dotm konnte nicht vom NAS kopiert werden.
    pause
    exit /b 1
)
```

Der VBA-Code in der Private Sub cmdNutzungsvereinbarungErstellen_Click() des Form_frmChorkleidung_DBListe wurde angepasst, sodass die Vorlage relativ zum Speicherort der Access-Datenbank gesucht wird.

Alt:

```text
Set objDocument = objWord.Documents.Add("H:\Joerg\Eigene Dokumente\Benutzerdefinierte Office-Vorlagen\Nutzungsvereinbarung_Chorkleidung.dotm")
```

Neu:

```text
Set objDocument = objWord.Documents.Add(CurrentProject.Path & "\Nutzungsvereinbarung_Chorkleidung.dotm")
```

Dabei wird der aktuelle Datenbankpfad als Ausgangspunkt verwendet. Es wird davon ausgegangen, dass sich die Datei `Nutzungsvereinbarung_Chorkleidung.dotm` im aktuellen Datenbank-Pfad befindet

Dadurch können Access-Datenbank und Word-Vorlage gemeinsam auf beliebige Arbeitsrechner kopiert werden.

---

# Ergebnis

Die Erstellung der Nutzungsvereinbarung funktioniert nun unabhängig vom verwendeten Rechner.

Voraussetzung ist lediglich, dass sich die Word-Vorlage gemeinsam mit der Access-Arbeitskopie in der vorgesehenen Verzeichnisstruktur befindet.

Die bisherige Abhängigkeit von einem fest verdrahteten lokalen Pfad wurde vollständig beseitigt.

---

# Dokumentation

Betroffene Komponenten:

- Access Mitgliederverwaltung
- Word-Vorlage `Nutzungsvereinbarung_Chorkleidung.dotm`

Keine Auswirkungen auf:

- Backend
- Frontend
- MariaDB
- EMC-INFRA

---

# Abschlussbewertung

BUG-001 wurde erfolgreich behoben.

Status:

```text
BUG-001
ABGESCHLOSSEN
```
