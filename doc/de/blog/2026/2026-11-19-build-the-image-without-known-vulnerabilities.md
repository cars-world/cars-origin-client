<!--
@license
Copyright (c) rljson

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Das Image ohne bekannte Schwachstellen bauen

## Motivation

- Ein Trivy-Scan des Images `cars-origin-client` fand 68 hohe oder kritische
  Schwachstellen mit Fix, 3 davon kritisch
- Sie stammten aus dem Basis-Image `nginx-unprivileged:1.27-alpine` auf
  Alpine 3.21: OpenSSL, curl, zlib, brotli, libxml2 und weitere

## Strategie

- Auf die aktuelle stabile Reihe von nginx wechseln, statt eine alte zu
  flicken
- Beim Bauen die Fixes von Alpine übernehmen, die nach dem Basis-Image
  erschienen sind
- Die Website und ihre Konfiguration bleiben, wie sie sind

## Dockerfile

- Die Laufzeit-Stufe beginnt mit `nginxinc/nginx-unprivileged:1.30-alpine`,
  der stabilen Reihe auf Alpine 3.24
- `apk upgrade --no-cache` läuft als root, bevor die Dateien kopiert
  werden; der Server läuft weiter als Nutzer 1000 auf Port 8080
- Die Build-Stufe mit Node.js und Vite bleibt, wie sie war

## Ergebnis

- Das Image baut; der Scan des nächsten Images zeigt die übrigen Befunde

## Offene Punkte

- Mit der Zeit kommen neue Schwachstellen hinzu; erst ein neu gebautes
  Image übernimmt ihre Fixes
