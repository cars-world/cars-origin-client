<!--
@license
Copyright (c) rljson

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Die Zeilen des Masters relativ zur Seite laden

## Motivation

- cars-origin-client ist die Client-Website der Seite origin,
  in der Kataloge ursprünglich erfasst werden
- Ihre Seite zeigt ihre eigene Hallo-Zeile und die Zeilen, mit denen der
  Master ihrer Seite antwortet; sie soll überall funktionieren, wo sie
  ausgeliefert wird, unter `/` oder unter einem Pfad
- Die Registry und die Stufen hatten neue Namen: eine Registry `crcars`
  für alle Stufen, und die Stufen `dev`, `test` und `prod`

## Strategie

- Den Master mit einer URL relativ zur Seite statt einer absoluten fragen
- nginx leitet `/api/` wie bisher an `MASTER_URL` weiter
- Ein Image je Version in `crcars` bauen

## Relative URL

- `helloLines` holt `api/hello` statt `/api/hello`
- Unter `/` ist das weiter `/api/hello`; unter einem Pfad wie
  `/origin-client/` löst es sich unterhalb dieses Pfads auf, so erreicht
  ein Proxy, der das Präfix entfernt, dasselbe nginx
- Die Zeile ohne Antwort nennt `api/hello`; die Tests folgen

## Image und Stufen

- Die README baut das Image mit
  `az acr build --registry crcars --image cars-origin-client:<version> .`
- Sie nennt die Stufen `dev`, `test` und `prod` und läuft in jeder als
  Container

## Ergebnis

- Die Seite zeigt beide Zeilen unter `/` und unter einem Pfad, geprüft im
  Browser und in den Tests

## Offene Punkte

- Die Seite zeigt nur Hallo-Zeilen; die Oberfläche folgt
