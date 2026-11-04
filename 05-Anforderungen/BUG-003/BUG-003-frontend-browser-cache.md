# BUG-003 – Veraltete Frontend-Version durch Browser-Cache

Status: In Arbeit

---

# Fehlerbeschreibung

Nach dem Deployment einer neuen Version des React-Frontends der EMC-Mitgliederverwaltung wurde im Browser teilweise weiterhin eine veraltete Frontend-Version angezeigt. In einzelnen Fällen erschien beim Aufruf der Anwendung zunächst nur eine vollständig weiße Seite.

Eine Aktualisierung mit `Strg + F5` führte dazu, dass die Anmeldemaske beziehungsweise die Anwendung korrekt erschien. Das Verhalten wurde sowohl in DEV als auch in PROD beobachtet. Eine weiße Seite trat außerdem beim lokalen Aufruf über die Vite-Entwicklungsumgebung auf.

Das Problem erschwert die zuverlässige Bereitstellung neuer Frontend-Versionen: Ein Benutzer kann nach einem Deployment nicht ohne Weiteres erkennen, ob der Browser den aktuellen Stand geladen hat.

---

# Ursache

Bei der Untersuchung wurde festgestellt, dass Nginx die HTML-Einstiegsdatei `index.html` bisher ohne ausdrücklichen `Cache-Control`-Header auslieferte. Der Server lieferte unter anderem `Last-Modified` und `ETag`.

Die fehlende explizite Cache-Steuerung wurde als möglicher Grund für die Wiederverwendung veralteter HTML-Inhalte bewertet. Da die von Vite erzeugten JavaScript- und CSS-Dateien versionsabhängige Dateinamen besitzen, kann eine veraltete `index.html` auf nicht mehr passende Dateien verweisen.

**Abgrenzung:** Die fehlende Cache-Regel ist nachgewiesen. Ob sie die alleinige Ursache aller beobachteten weißen Seiten war, ist nicht abschließend belegt. Insbesondere das ähnliche Verhalten beim lokalen Start über Vite ist damit nicht vollständig erklärt.

---

# Umsetzung

Für die nächste Frontend-Version `1.2.2-SNAPSHOT` wurde in den drei Nginx-Konfigurationen eine gesonderte Regel für die HTML-Einstiegsdatei ergänzt:

- `docker/nginx-dev.conf`
- `docker/nginx-prod.conf`
- `docker/nginx-local.conf`

```nginx
# HTML-Einstiegsdatei immer auf Aktualität prüfen
location = /index.html {
    add_header Cache-Control "no-cache" always;
}
```

Die bestehende SPA-Routenbehandlung wurde in den drei Konfigurationen auf folgende Form gebracht:

```nginx
location / {
    try_files $uri $uri/ /index.html;
}
```

Damit wird die HTML-Einstiegsdatei bei erneuten Anforderungen auf Aktualität geprüft. Die Cache-Behandlung anderer statischer Dateien wird durch diese Regel nicht pauschal geändert. Die Backend-Proxy-Adressen in DEV und PROD bleiben unverändert.

**Versions- und Git-Stand:**

- Entwicklungsbranch: `fix/frontend-cache-control`
- Frontend-Version: `1.2.2-SNAPSHOT`
- Commit: `f4f0bc2` – `fix(frontend): prevent stale HTML caching in nginx`
- Branch wurde auf GitHub veröffentlicht; noch kein Merge nach `master`.

Die Frontend-Anwendungsversion `1.2.2-SNAPSHOT` wurde für `linux/arm64` als Docker-Image erstellt und in DEV bereitgestellt. Der feste Docker-Image-Tag der DEV-Umgebung lautet `emc-mitglieder-frontend:dev` und ist von der Anwendungsversion zu unterscheiden. Vor einem Rollback ist zu prüfen, ob das vorherige Image noch verfügbar und eindeutig identifizierbar ist.

Die vorbereitete `nginx-prod.conf` ist bislang ausschließlich Bestandteil des Entwicklungsbranches. **Ein PROD-Deployment von Version `1.2.2` ist noch nicht erfolgt.**

---

# Ergebnis

**Technische Prüfungen:**

- 65 Frontend-Unit-Tests mit Vitest erfolgreich.
- Vite-Produktionsbuild erfolgreich.
- Docker-Image für `linux/arm64` erfolgreich gebaut.
- Nginx-Konfiguration im Docker-Netzwerk `emc_net` erfolgreich mit `nginx -t` geprüft.
- HTTP-Antworten für `/index.html` und `/members` im temporären Testcontainer jeweils mit `200 OK` und `Cache-Control: no-cache` bestätigt.
- DEV-Deployment erfolgreich; laufende Image-ID `sha256:04b0730545fa4bc2f4295da9fb49052e4de33b6c77ddc8056039a1823243b8e4` bestätigt.

**Browserprüfungen:**

- Beim ersten Aufruf nach dem DEV-Deployment trat auf dem bisherigen Rechner zunächst erneut eine weiße Seite auf; `Strg + F5` stellte die Anwendung wieder her.
- Anschließende Aktualisierungen mit `Strg + R` funktionierten.
- Nach vollständigem Neustart von Edge erschien auf diesem Rechner die Anmeldemaske ohne erzwungene Aktualisierung.
- Auf einem zweiten Rechner, auf dem DEV `1.2.1-SNAPSHOT` zuvor nicht verwendet worden war, erschien die Anmeldemaske sofort; die Systeminformationen zeigten die korrekte Frontend-Version `1.2.2-SNAPSHOT`.

Die Tests zeigen, dass die neue Cache-Regel technisch wirksam ist und die Anwendung in DEV ordnungsgemäß ausgeliefert wird.

**DEV-Beobachtung, Stand 04.11.2026:** Seit den ersten Tests nach der Bereitstellung von `1.2.2-SNAPSHOT` wurde die weiße Browserseite nach Rückmeldung aus dem Betrieb bislang nicht erneut beobachtet. Dies spricht für die Wirksamkeit der Korrektur, ist aber noch kein abschließender Nachweis für eine dauerhafte Fehlerbehebung.

---

# Dokumentation

Betroffene Komponenten:

- React-/Vite-Frontend
- Nginx-Konfigurationen für LOCAL, DEV und PROD
- Docker-Image und Deployment der DEV-Umgebung

Keine fachlichen Änderungen an:

- Java-/Spring-Boot-Backend
- Datenbankschema und Datenbestand

Noch ausstehend:

- DEV über mehrere Tage beobachten, insbesondere nach Browser-Neustarts und auf bereits zuvor verwendeten Rechnern.
- Bei erneutem Auftreten die Browser-Netzwerkanfragen und das Laden der JavaScript-Dateien gezielt untersuchen.
- Nach erfolgreicher DEV-Abnahme den Entwicklungsbranch nach `master` übernehmen.
- Release-Version `1.2.2` erstellen und die Korrektur in PROD bereitstellen und prüfen.
- Fehlerbehebung im `CHANGELOG.md` der Version `1.2.2` erfassen.

---

# Abschlussbewertung

BUG-003 ist technisch bearbeitet und mit Version `1.2.2-SNAPSHOT` in DEV umgesetzt. Die technischen Prüfungen waren erfolgreich; nach den anfänglichen Browserproblemen wurde bislang kein erneutes Auftreten gemeldet. Die Beobachtungsphase läuft weiter.

Eine abschließende Fehlerbehebung kann erst nach bestätigter Wirksamkeit und erfolgreichem PROD-Deployment festgestellt werden.

Status:

```text
BUG-003
IN ARBEIT
```
