<!--
@license
Copyright (c) rljson

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Bei jedem Push auf main nach dev ausrollen

## Motivation

- Bisher baute ein Betreiber das Image von `cars-origin-client` von Hand mit
  `az acr build` und schrieb seinen Tag in `cars-deployment`
- Eine gemergte Änderung soll `dev` innerhalb von Minuten erreichen, ohne
  Handgriff und ohne in GitHub gespeichertes Secret

## Strategie

- Die Pipeline an einem Ort halten: der wiederverwendbare Workflow `build`
  von [cars-deployment](https://github.com/cars-world/cars-deployment)
- Dieses Repo ruft ihn nur bei einem Push auf `main` mit seinem App-Namen
  auf
- Über OIDC bei Azure anmelden; der Workflow braucht kein gespeichertes
  Secret

## Der Workflow deploy.yaml

- `.github/workflows/deploy.yaml` läuft bei jedem Push auf `main` und ruft
  `build.yaml@main` von `cars-deployment` mit `app: origin-client` auf
- Er erlaubt `id-token: write` für die Anmeldung und liest die Variablen
  `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID` und
  `CARS_DEPLOY_APP_ID` der Organisation `cars-world`
- `build` testet die App, baut das Image
  `crcars.azurecr.io/cars-origin-client:<version>-<kurzer Commit>` und schreibt den Tag
  in die Werte von `dev`; Argo CD synchronisiert `dev`
- Das README sagt, dass ein Push ausrollt, und baut von Hand nur für einen
  Versuch; `index.jsonc` nennt `cars-deployment` unter `uses`

## Ergebnis

- Ein Merge in `main` baut `origin-client` und rollt es nach `dev` aus
- `pnpm test` läuft durch

## Offene Punkte

- Der erste Lauf folgt dem Release dieser Änderung; Azure nimmt die
  Anmeldung nur von `main` an
- `test` und `prod` folgen über den Workflow `promote` von
  `cars-deployment`
