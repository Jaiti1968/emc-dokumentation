# BL-008 Frontend PROD Image Deployment

Status: Geplant

Priorität: Hoch

Roadmap-Bezug: P1 – Frontend Betriebs- und Deploymentstandardisierung

---

# 1. Ziel

Umstellung des Produktivbetriebs des Frontends von einem lokalen Build auf ein versioniertes Docker-Image.

Ziel ist eine standardisierte, reproduzierbare und nachvollziehbare Deployment-Strategie analog zum bereits etablierten Backend-Betrieb.

---

# 2. Ausgangssituation

Aktuell wird das Frontend in der Produktivumgebung direkt aus dem lokalen Projektverzeichnis gebaut und bereitgestellt.

Dadurch ergeben sich folgende Nachteile:

- Build und Deployment sind gekoppelt
- Deployments sind nur eingeschränkt reproduzierbar
- eine eindeutige Versionierung des ausgelieferten Frontends ist nicht gewährleistet
- Rollbacks sind erschwert
- DEV- und PROD-Betrieb verwenden unterschiedliche Deployment-Muster

Das Backend verwendet bereits ein imagebasiertes Deployment-Modell.

---

# 3. Motivation

Mit der NAS-Betriebsstandardisierung wurde eine einheitliche Containerplattform geschaffen.

Das Frontend soll sich an denselben Betriebsprinzipien orientieren wie das Backend:

- reproduzierbare Builds
- versionierte Artefakte
- nachvollziehbare Releases
- vereinfachte Rollbacks
- konsistenter DEV-/PROD-Betrieb

---

# 4. Fachliche Anforderungen

## FA-008-01 Unverändertes Nutzerverhalten

Die Umstellung darf keine Auswirkungen auf die fachliche Funktionalität des Frontends haben.

Alle bestehenden Funktionen müssen unverändert verfügbar bleiben.

---

## FA-008-02 Transparenter Übergang

Die Umstellung soll für Endanwender vollständig transparent erfolgen.

---

# 5. Technische Anforderungen

## TA-008-01 Imagebasierter PROD-Betrieb

Das Frontend soll in PROD aus einem versionierten Docker-Image bereitgestellt werden.

---

## TA-008-02 Reproduzierbare Builds

Frontend-Releases müssen reproduzierbar erzeugt werden können.

---

## TA-008-03 Versionierung

Die ausgelieferte Frontend-Version muss eindeutig nachvollziehbar sein.

---

## TA-008-04 Konsistentes Deployment-Modell

Das Deployment-Modell soll sich an den etablierten Backend-Prozessen orientieren.

---

## TA-008-05 Rollback-Fähigkeit

Die gewählte Lösung soll spätere Rollbacks auf frühere Versionen erleichtern.

---

## TA-008-06 Einheitliches DEV-/PROD-Image-Modell

Frontend DEV und Frontend PROD sollen künftig nach demselben Grundprinzip betrieben werden.

Dabei gilt:

```text
DEV  = X.Y.Z-SNAPSHOT Image
PROD = X.Y.Z Image
```

Dieses Modell entspricht dem bereits etablierten Backend-Vorgehen.

Ziel ist ein konsistentes Release- und Deploymentmodell für Backend und Frontend.

---

# 6. Betriebsanforderungen

## BA-008-01 DEV weiterhin funktionsfähig und imagebasiert

Die bestehende DEV-Umgebung darf durch die Umstellung nicht beeinträchtigt werden.

DEV soll weiterhin über ein imagebasiertes Deployment betrieben werden.

Dabei sind Snapshot-Versionen zu verwenden:

```text
X.Y.Z-SNAPSHOT
```

---

## BA-008-02 PROD standardisieren

Die Produktivumgebung soll künftig ausschließlich auf bereitgestellten Images basieren.

---

## BA-008-03 Dokumentierter Deployment-Prozess

Der Deployment-Prozess muss nachvollziehbar dokumentiert werden.

---

# 7. Nichtziele

Nicht Bestandteil dieser Anforderung sind:

- Änderungen an der Frontend-Funktionalität
- UI-Anpassungen
- neue Features
- Runtime- oder Environment-Anzeigen im Frontend
- CI/CD-Einführung
- automatische Build-Pipelines

---

# 8. Dokumentationsauswirkungen

## Frontend Repository

Mögliche Anpassungen:

- README
- Build- und Deployment-Dokumentation

---

## EMC-INFRA

Mögliche Anpassungen:

- Stack Inventory
- Deployment-Dokumentation
- Betriebsdokumentation

---

## EMC-DOKUMENTATION

Anpassungen:

- Projektstatus
- Backlog
- Roadmap
- Ergebnisdokumentation

Zusätzlich:

- docs/02-deployment.md

Das Deployment-Dokument ist an das neue imagebasierte Frontend-Deploymentmodell anzupassen.

---

# 9. Abnahmekriterien

Die Anforderung gilt als erfüllt, wenn:

- das Frontend in PROD aus einem Docker-Image betrieben wird
- das Deployment reproduzierbar durchgeführt werden kann
- die fachliche Funktion unverändert bleibt
- die Dokumentation aktualisiert wurde
- ein erfolgreiches PROD-Deployment nachgewiesen wurde
- ein Ergebnisdokument erstellt wurde
- DEV läuft mit einem `X.Y.Z-SNAPSHOT` Image
- PROD läuft mit einem `X.Y.Z` Image
- Frontend- und Backend-Versionierungsmodell sind konsistent

---

# 10. Abhängigkeiten

Voraussetzungen:

- bestehendes Frontend-Dockerfile
- bestehende DEV-/PROD-Umgebung
- abgeschlossene NAS-Betriebsstandardisierung

Keine fachlichen Abhängigkeiten.

---

# 11. Risiken

Gering.

Mögliche Risiken:

- Unterschiede zwischen lokalem Build und Image-Build
- Konfigurationsabweichungen zwischen DEV und PROD
- Fehlerhafte Versionierung

Diese Risiken sind während der Umsetzung explizit zu prüfen.

---

# 12. Erwartetes Ergebnis

Nach Umsetzung verfügt die EMC Mitgliederverwaltung über:

- imagebasierten Frontend-PROD-Betrieb
- reproduzierbare Frontend-Releases
- standardisierte Deployment-Prozesse
- bessere Nachvollziehbarkeit von Releases
- verbesserte Rollback-Möglichkeiten

---

Anforderungs-ID: BL-008

Titel: Frontend PROD Image Deployment

Status: Geplant

Priorität: Hoch
