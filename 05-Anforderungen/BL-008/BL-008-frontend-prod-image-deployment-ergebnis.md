Datei: `BL-008-frontend-prod-image-deployment-ergebnis.md`

Ablagepfad:

`05-Anforderungen/BL-008/BL-008-frontend-prod-image-deployment-ergebnis.md`

Stand: 15.06.2026

---

# BL-008 – Frontend PROD Image Deployment

## Ergebnisdokument

---

# 1. Zielsetzung

Ziel von BL-008 war die Umstellung des Frontend-PROD-Deployments auf einen image-basierten Betrieb.

Bislang wurde das Frontend PROD als Sonderfall betrieben:

* NGINX Standardimage
* Bind-Mount des dist-Verzeichnisses
* Bind-Mount der nginx-Konfiguration

Dieses Verfahren wich vom Backend-Deploymentmodell ab und erschwerte Rollback, Versionierung und Standardisierung.

Ziel war die Einführung eines einheitlichen Deploymentmodells für Frontend und Backend.

---

# 2. Ausgangssituation

Frontend DEV wurde bereits image-basiert betrieben.

Frontend PROD verwendete hingegen:

```text
nginx:alpine
+
dist-Verzeichnis
+
nginx.conf
```

als Bind-Mounts.

Dadurch ergaben sich mehrere Nachteile:

* unterschiedliche Betriebsmodelle für DEV und PROD
* fehlende Versionierung des Frontend-Artefakts
* erschwerte Rollbackfähigkeit
* erhöhte Betriebs- und Dokumentationskomplexität

---

# 3. Durchgeführte Arbeiten

## 3.1 Dockerfile-Standardisierung

Das bestehende Dockerfile wurde für unterschiedliche Deployment-Ziele nutzbar gemacht.

Eingeführt wurde:

```text
ARG NGINX_CONF
```

zur Auswahl der gewünschten nginx-Konfiguration.

Unterstützte Varianten:

```text
nginx-local.conf
nginx-dev.conf
nginx-prod.conf
```

---

## 3.2 Einführung ARM64-kompatibler Builds

Während der Umsetzung wurde festgestellt, dass die NAS-Plattform ARM64 verwendet.

Docker Images müssen daher mit:

```bash
--platform linux/arm64
```

gebaut werden.

Andernfalls tritt der Fehler:

```text
exec /docker-entrypoint.sh: exec format error
```

auf.

---

## 3.3 Snapshot-/Release-Strategie

Für Frontend-Deployments wurde eine konsistente Versionierungsstrategie eingeführt.

DEV:

```text
X.Y.Z-SNAPSHOT
```

PROD:

```text
X.Y.Z
```

Beispiel:

```text
DEV:
1.1.2-SNAPSHOT

PROD:
1.1.1
```

Damit entspricht das Frontend nun dem bereits etablierten Backend-Standard.

---

## 3.4 Frontend-Versionsanzeige korrigiert

Während der Tests wurde festgestellt, dass die Frontend-Versionsanzeige weiterhin Snapshot-Versionen anzeigte.

Ursache:

```text
package.json
```

wird zur Build-Zeit ausgewertet.

Für Release-Builds muss daher vor dem Build eine Release-Version gesetzt werden.

Die Dokumentation wurde entsprechend ergänzt.

---

## 3.5 Einführung Image-Archivierung

Für Frontend-Images wurde ein lokales Archiv eingeführt:

```text
docker-image-archive/
```

Beispiel:

```text
docker-image-archive/
├── emc-mitglieder-frontend-1.1.1.tar
└── emc-mitglieder-frontend-1.1.1-SNAPSHOT.tar
```

Ziele:

* Nachvollziehbarkeit
* Rollbackfähigkeit
* reproduzierbare Deployments

Das Verzeichnis wurde in `.gitignore` aufgenommen.

---

## 3.6 PROD-Migration

Frontend PROD wurde erfolgreich auf image-basiertes Deployment umgestellt.

Vorher:

```text
nginx:alpine
+
Bind-Mounts
```

Nachher:

```text
emc-mitglieder-frontend:1.1.1
```

Die Portainer-Stackdefinition wurde entsprechend angepasst.

---

## 3.7 Deployment-Governance

Während der Umsetzung wurde deutlich, dass mehrere Konfigurationsebenen konsistent gehalten werden müssen.

Betroffen sind:

```text
EMC-INFRA
→ compose

NAS
→ /volume1/docker/compose

Portainer
→ Stack Definition
```

Die Governance wurde in die Betriebsdokumentation übernommen.

---

# 4. Testergebnisse

## DEV

Erfolgreich geprüft:

* Image Build
* Image Start
* nginx-Konfiguration
* Backend-Anbindung
* Versionsanzeige
* Frontend-Funktionalität

Ergebnis:

```text
FE v1.1.2-SNAPSHOT
```

---

## PROD

Erfolgreich geprüft:

* Image Build
* Image Transfer
* docker load
* Portainer Redeploy
* nginx-Konfiguration
* Backend-Anbindung
* Versionsanzeige
* Frontend-Funktionalität

Ergebnis:

```text
FE v1.1.1
```

---

## Qualitätssicherung

Frontend Unit Tests:

```text
7 Testdateien
64 Tests
64 erfolgreich
```

Build:

```text
npm run build
```

erfolgreich.

---

# 5. Dokumentationsanpassungen

Aktualisiert wurden:

## Frontend Repository

* README.md
* README_DEVELOPER.md

## EMC-INFRA

* stack-inventory.md
* infra-governance.md
* bare-metal-nas-recovery.md

## EMC-DOKUMENTATION

* 02-deployment.md
* 02.30-backlog.md
* 02.40-entscheidungen.md

---

# 6. Ergebnis

BL-008 wurde erfolgreich abgeschlossen.

Frontend DEV und Frontend PROD verwenden nun dasselbe image-basierte Deploymentmodell wie das Backend.

Erreichte Verbesserungen:

* einheitliches Deploymentmodell
* Snapshot-/Release-Strategie
* ARM64-kompatible Images
* reproduzierbare Deployments
* versionierte Frontend-Artefakte
* standardisierte Rollbackfähigkeit
* konsolidierte Betriebsdokumentation
* verbesserte Deployment-Governance

Die EMC Mitgliederverwaltung verfügt damit über einen konsistenten und dokumentierten Frontend-Deploymentprozess für DEV- und PROD-Betrieb.

---

# 7. Folgeaktivitäten

Aus der Umsetzung ergab sich ein Folgevorhaben:

```text
BL-019 Frontend Anzeige Laufzeitumgebung
```

Ziel:

Anzeige der tatsächlichen Backend-Laufzeitumgebung:

```text
LOCAL
DEV
PROD
```

auf Basis der bereits durch BL-007 bereitgestellten Informationen aus:

```text
/api/system/info
```

Dadurch soll die eindeutige Identifikation der aktiven Zielumgebung weiter verbessert werden.

