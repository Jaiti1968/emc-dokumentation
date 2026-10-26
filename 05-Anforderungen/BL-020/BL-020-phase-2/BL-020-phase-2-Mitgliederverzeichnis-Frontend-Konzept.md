# BL-020 Phase 2 – Mitgliederverzeichnis – Frontend-Konzept

## Status

Entwurf

## Zugehörige Anforderung

BL-020 Reporting Umsetzung

## Zugehörige Phase

BL-020 Phase 2 – Mitgliederverzeichnis

---

# 1. Zielsetzung

Dieses Dokument beschreibt das Frontend-Konzept für den ersten produktiven Reporting-Baustein:

```text
Mitgliederverzeichnis
```

Ziel ist die Definition der Benutzeroberfläche, Bedienlogik und Interaktionsmuster innerhalb des in Phase 1 definierten Reporting-Grundgerüsts.

Das Konzept soll gleichzeitig als Referenz für zukünftige Reporting-Module dienen.

---

# 2. Einordnung in die Reporting-Architektur

Gemäß BL-020 Phase 1 entsteht im Hauptmenü:

```text
Mitglieder
Reporting
Administration
```

Das Mitgliederverzeichnis ist der erste Bericht innerhalb des Bereichs:

```text
Reporting
```

Navigation:

```text
Reporting
└── Mitgliederverzeichnis
```

---

# 3. Grundprinzipien

## Einheitliche Reporting-Oberfläche

Das Mitgliederverzeichnis soll bereits die späteren Standards für weitere Reports etablieren.

Daher wird die Oberfläche in klar getrennte Bereiche aufgeteilt:

```text
Berichtsauswahl
↓
Filterbereich
↓
Aktionen
↓
Ergebnisbereich
```

---

## Sofortige Verständlichkeit

Die Bedienung soll ohne Schulung möglich sein.

Der Anwender soll:

- Bericht auswählen
- Filter setzen
- Ergebnis anzeigen
- Export starten

können.

---

## Desktop- und Tablet-Kompatibilität

Die Oberfläche muss sowohl auf:

```text
Laptop
Tablet
```

nutzbar bleiben.

Filter und Aktionen sollen daher oberhalb der Ergebnisliste angeordnet werden.

---

# 4. Routing

## URL

Vorgeschlagen:

```text
/reporting/members-directory
```

---

## Berechtigung

Aktuelle Phase:

```text
ADMIN
```

Nicht berechtigte Benutzer erhalten:

```text
403 Forbidden
```

bzw. sehen keinen Menüeintrag.

---

# 5. Seitenaufbau

## Gesamtstruktur

```text
┌──────────────────────────────┐
│ Reporting                    │
│ Mitgliederverzeichnis        │
├──────────────────────────────┤
│ Variantenwahl                │
├──────────────────────────────┤
│ Filterbereich                │
├──────────────────────────────┤
│ Aktionen                     │
├──────────────────────────────┤
│ Ergebnisbereich              │
└──────────────────────────────┘
```

---

# 6. Variantenwahl

## Zweck

Auswahl der gewünschten Listenvariante.

---

## Darstellung

Empfohlen:

```text
Variante:

( ) Standard
( ) Kompakt
( ) Fortlaufend
( ) Kammerchor
```

Alternativ:

```text
Dropdown
```

Radiobuttons werden bevorzugt, da alle Varianten gleichzeitig sichtbar bleiben.

---

## Standardwert

```text
Standard
```

---

## Verhalten

Beim Wechsel der Variante:

- werden Standardfilter gesetzt
- wird die Ergebnisdarstellung angepasst
- erfolgt keine automatische Ausführung

Der Benutzer startet die Auswertung bewusst.

---

# 7. Filterbereich

## Aufbau

```text
Freitext

Mitgliedsstatus

Stimme

Kammerchor
```

---

## Freitext

Beschriftung:

```text
Suche
```

Platzhalter:

```text
Name, Mitgliedsnummer, Ort, E-Mail ...
```

---

## Mitgliedsstatus

Komponente:

```text
MultiSelect
```

Grund:

Mehrere Status sollen gleichzeitig auswählbar sein.

---

## Stimme

Komponente:

```text
MultiSelect
```

Beispiel:

```text
Sopran
Alt
Tenor
Bass
```

---

## Kammerchor

Komponente:

```text
Dropdown
```

Werte:

```text
Alle
Nur Kammerchor
Ohne Kammerchor
```

---

## Variante Kammerchor

Bei Auswahl der Variante:

```text
Kammerchor
```

wird automatisch gesetzt:

```text
Nur Kammerchor
```

Der Benutzer kann dies nicht ändern.

---

# 8. Aktionsbereich

## Buttons

Vorgesehen:

```text
Anzeigen

Zurücksetzen

PDF Export

Excel Export
```

CSV wird zunächst nicht im Frontbereich priorisiert.

---

## Anzeigen

Startet die Berichtsabfrage.

---

## Zurücksetzen

Setzt alle Filter auf Standardwerte zurück.

---

## PDF Export

Exportiert die aktuelle Auswertung.

---

## Excel Export

Exportiert die aktuelle Auswertung.

---

# 9. Ergebnisbereich

## Zweck

Anzeige der erzeugten Auswertung im Browser.

---

## Grundprinzip

Der Browser dient als primäre Vorschau.

Der Benutzer soll die Daten vor einem Export prüfen können.

---

# 10. Ergebnisdarstellung und Layoutvarianten

## Grundsatz

Die fachliche Datenbasis ist in allen Varianten grundsätzlich identisch.

Die Varianten unterscheiden sich nicht durch unterschiedliche fachliche Inhalte, sondern durch:

- Layout
- Gruppierungsverhalten
- Seitenumbruchverhalten bei PDF- und Druckausgaben

Die Browser-Vorschau verwendet grundsätzlich dieselbe Datenbasis wie die späteren Exportformate.

---

## Verfügbare Felder

Das Mitgliederverzeichnis basiert auf folgenden Feldern:

```text
Mitgliedsnummer
Nachname
Vorname
PLZ
Ort
Straße
Geburtsdatum
Telefon privat
Telefon geschäftlich
Mobiltelefon
E-Mail
```

Welche Felder tatsächlich sichtbar sind, kann später exportformatspezifisch angepasst werden.

Die fachliche Datenbasis bleibt jedoch identisch.

---

## Standard

Darstellung:

```text
2-zeilig
```

Merkmale:

- bessere Lesbarkeit
- vollständige Informationsdarstellung
- geeignet für Ausdrucke und Vorstandsunterlagen

Verhalten bei Gruppierungswechsel:

```text
Automatischer Seitenwechsel
```

---

## Kompakt

Darstellung:

```text
1-zeilig
```

Merkmale:

- platzsparende Darstellung
- mehr Mitglieder pro Seite
- geeignet für Arbeitslisten

Verhalten bei Gruppierungswechsel:

```text
Automatischer Seitenwechsel
```

---

## Fortlaufend

Darstellung:

```text
1-zeilig
```

Merkmale:

- kontinuierliche Liste
- maximale Informationsdichte
- besonders geeignet für Export und Nachschlagezwecke

Verhalten bei Gruppierungswechsel:

```text
Kein automatischer Seitenwechsel
```

---

## Kammerchor

Die Kammerchor-Variante ist keine eigenständige Darstellungsform.

Sie stellt fachlich eine Einschränkung auf Kammerchor-Mitglieder dar.

Für Kammerchor können grundsätzlich dieselben Layoutvarianten verwendet werden:

```text
Standard
Kompakt
Fortlaufend
```

Die Unterschiede ergeben sich ausschließlich aus der Gruppierung und Filterung.

---

# 11. Gruppierungsdarstellung

## Fachlicher Grundsatz

Die Gruppierung des Mitgliederverzeichnisses erfolgt abhängig vom Mitgliedsstatus.

Dabei gilt:

```text
Aktive Mitglieder
→ zusätzliche Gruppierung nach Stimme

Alle anderen Mitgliedsstatus
→ keine Stimmgruppierung
```

Hintergrund:

Die Stimme besitzt fachliche Relevanz ausschließlich für aktive Sänger.

Bei passiven Mitgliedern, Ehrenmitgliedern oder sonstigen Statusgruppen wird keine Stimmgruppierung vorgenommen.

---

## Standard

Gruppierung:

```text
Mitgliedsstatus
↓

bei aktiven Mitgliedern zusätzlich:

Stimme
```

Beispiel:

```text
Aktive Mitglieder

  1. Tenor
  ...

  2. Tenor
  ...

  1. Bass
  ...

  2. Bass
  ...

Passive Mitglieder
...

Ehrenmitglieder
...
```

Bei jedem Gruppierungswechsel erfolgt in PDF- und Druckausgaben ein automatischer Seitenwechsel.

---

## Kompakt

Die Gruppierungslogik entspricht der Standardvariante.

Bei jedem Gruppierungswechsel erfolgt in PDF- und Druckausgaben ein automatischer Seitenwechsel.

---

## Fortlaufend

Die Gruppierungslogik entspricht der Standardvariante.

Es erfolgt jedoch kein automatischer Seitenwechsel bei Gruppierungswechseln.

Die Ausgabe erfolgt fortlaufend.

---

## Kammerchor

Da Kammerchor-Mitglieder ausschließlich aktive Mitglieder sein können, erfolgt die Gruppierung direkt nach Stimme.

Gruppierung:

```text
1. Tenor

2. Tenor

1. Bass

2. Bass
```

Je nach gewählter Layoutvariante erfolgt die Ausgabe als:

```text
Standard

Kompakt

Fortlaufend
```

---

## Sortierung innerhalb der Gruppen

Innerhalb jeder Gruppierung erfolgt die Sortierung einheitlich nach:

```text
Nachname
↓
Vorname
```

Diese Sortierung gilt für:

- Standard
- Kompakt
- Fortlaufend
- Kammerchor

---

# 12. Ergebnisinformationen

Oberhalb der Tabelle:

```text
127 Mitglieder gefunden
```

Optional:

```text
Aktive Filter:
Aktiv, Sopran, Kammerchor
```

---

# 13. Pagination

## Phase 2

Nicht erforderlich.

Begründung:

Die Mitgliederanzahl des Vereins ist überschaubar.

Eine vollständige Anzeige verbessert die Druck- und Exportorientierung.

---

## Zukunft

Bei Bedarf später nachrüstbar.

---

# 14. Ladezustände

## Bericht wird erzeugt

Anzeige:

```text
Lade Mitgliederverzeichnis ...
```

plus Spinner.

---

## Keine Treffer

Anzeige:

```text
Keine Mitglieder gefunden.
```

---

## Fehler

Anzeige:

```text
Mitgliederverzeichnis konnte nicht geladen werden.
```

plus fachliche Fehlermeldung.

---

# 15. Komponentenmodell

## Neue Seitenkomponente

```text
MembersDirectoryReportPage
```

---

## Neue Filterkomponente

```text
MembersDirectoryFilterPanel
```

---

## Variantenkomponente

```text
ReportVariantSelector
```

---

## Ergebniskomponente

```text
MembersDirectoryResultTable
```

---

## Aktionskomponente

```text
ReportActionBar
```

---

# 16. Wiederverwendbare Reporting-Komponenten

Aus Phase 1 sollen möglichst generische Komponenten entstehen.

Zielbild:

```text
ReportPageLayout

ReportFilterPanel

ReportActionBar

ReportResultTable
```

Diese Komponenten können später ebenfalls verwendet werden für:

- Geburtstagswesen
- Zugehörigkeitswesen
- Kontrollberichte
- Statistik

---

# 17. Responsive Verhalten

## Desktop

Filter nebeneinander.

---

## Tablet

Filter untereinander.

Buttons umbrechen automatisch.

---

## Mobilgerät

Kein Optimierungsziel dieser Phase.

Die Nutzung erfolgt primär auf:

```text
Tablet
Laptop
```

---

# 18. Accessibility

Grundanforderungen:

- Tastaturbedienung
- Fokussteuerung
- Beschriftete Formularelemente
- ausreichende Kontraste

---

# 19. Nicht Bestandteil der Phase

Nicht vorgesehen:

- Diagramme
- Dashboards
- Speicherung persönlicher Filter
- Favoriten
- automatische Berichtsaktualisierung
- konfigurierbare Spalten
- Drag-and-Drop-Anpassungen

---

# 20. Frontend-Entscheidungsempfehlung

Für Phase 2 wird empfohlen:

```text
Reporting-Seite
+
Variantenwahl
+
Filterbereich
+
Vorschautabelle
+
PDF Export
+
Excel Export
```

Die Browser-Vorschau wird als primäre Arbeitsoberfläche betrachtet.

PDF und Excel dienen als Ausgabemedien derselben Auswertung.

Dadurch entsteht bereits im ersten produktiven Report ein konsistenter Standard für alle zukünftigen Reporting-Funktionen.

---

# 21. Abschlussbewertung

Das Frontend-Konzept des Mitgliederverzeichnisses nutzt die in Phase 1 geschaffene Reporting-Struktur konsequent aus.

Die Oberfläche bleibt:

- einfach verständlich
- für Vereinsanwender geeignet
- wiederverwendbar
- kompatibel mit zukünftigen Reports

und bildet damit den Frontend-Standard für die weitere Reporting-Migration.
