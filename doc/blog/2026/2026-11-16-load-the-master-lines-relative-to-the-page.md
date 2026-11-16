<!--
@license
Copyright (c) rljson

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Load the lines of the master relative to the page

## Motivation

- cars-origin-client is the client website of the origin side,
  where catalogs are captured originally
- Its page shows its own hello line and the lines the master of its side
  answers; it should work wherever it is served, at `/` or under a path
- The registry and the stages had new names: one registry `crcars` for
  all stages, and the stages `dev`, `test` and `prod`

## Strategy

- Ask the master with a URL relative to the page instead of an absolute
  one
- Keep nginx forwarding `/api/` to `MASTER_URL` as before
- Build one image per version into `crcars`

## Relative URL

- `helloLines` fetches `api/hello` instead of `/api/hello`
- At `/` this is still `/api/hello`; under a path such as
  `/origin-client/` it resolves below that path, so a proxy that strips
  the prefix reaches the same nginx
- The line shown without an answer names `api/hello`; the tests follow

## Image and stages

- The README builds the image with
  `az acr build --registry crcars --image cars-origin-client:<version> .`
- It names the stages `dev`, `test` and `prod` and runs as a container in
  each

## Result

- The page shows both lines at `/` and under a path, checked in the
  browser and in the tests

## Open Points

- The page shows hello lines only; the user interface follows
