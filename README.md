# Öffentliche DATEX-Daten für uMap

Dieses Repository verarbeitet ausschließlich öffentliche MobiData-BW-/DATEX-II-Quellen für Baustellen und Umleitungen im Landkreis Sigmaringen. Es enthält keine manuell gepflegten uMap-Ebenen, deren Adressen oder deren Sicherungen. Es übernimmt keine Git-Historie eines anderen Repositorys.

## Betrieb

Der Workflow „Baustellendaten aktualisieren“ läuft geplant alle 30 Minuten (Minute 17 und 47 UTC) und kann unter Actions manuell gestartet werden. GitHub kann geplante Läufe verzögern oder auslassen. Nach längerer Inaktivität den Workflowstatus kontrollieren. Bei Quellfehlern bleiben die vorherigen veröffentlichten Dateien bestehen. Zeitstempel: data/status.json bzw. metadata.generated im GeoJSON.

Öffentliche Anzeigequellen:

- Baustellen: https://raw.githubusercontent.com/googie0815/umap-datex-public/main/data/roadworks-datex.geojson
- Umleitungen: https://raw.githubusercontent.com/googie0815/umap-datex-public/main/data/rerouting-datex.geojson

## Cloudflare aktualisieren

Im bestehenden Worker patient-pine-10b1 den vollständigen Inhalt von cloudflare/Cloudflare_uMap_Code.txt einsetzen und Deploy wählen. Der Worker bezieht die Baustellendaten anschließend aus diesem Repository. Die bisherige Worker-Adresse /roadworks-datex.geojson und die Icon-Endpunkte bleiben unverändert.

Umleitungs-Ebenen mit direktem GitHub-Link müssen auf die oben genannte neue URL umgestellt werden. GeoJSON als Format verwenden. GitHub, Cloudflare und Browser können Daten zwischenspeichern.

## Verarbeitung

scripts/generate.mjs nutzt die gebündelte scripts/engine.mjs für Baustellen, scripts/generate-rerouting.py verarbeitet Umleitungen. Landkreisgrenze und SVG-Icons kommen weiterhin aus dem öffentlichen Repository umap-icons. Quellen und Aufbereitung entsprechen dem bestehenden DATEX-Aufbau; zukünftige Meldungen bleiben sichtbar, abgelaufene und ausgesetzte Umleitungen werden ausgelassen. Die Zuordnung der Sperrungsbezeichnung erfolgt über situation_id. Es wird keine Eignung der Umleitung für Großraum- und Schwertransporte zugesichert.

Prüfung: python3 scripts/test-rerouting.py. Aufbereitung: node scripts/generate.mjs und anschließend python3 scripts/generate-rerouting.py. Benötigt Node 22 oder neuer und Python mit Zeitzonendaten, keine externen Python-Pakete.
