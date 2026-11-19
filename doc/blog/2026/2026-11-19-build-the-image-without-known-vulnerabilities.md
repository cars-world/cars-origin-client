<!--
@license
Copyright (c) rljson

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Build the image without known vulnerabilities

## Motivation

- A Trivy scan of the image `cars-origin-client` found 68 high or critical
  vulnerabilities with a fix, 3 of them critical
- They came from the base image `nginx-unprivileged:1.27-alpine` on
  Alpine 3.21: OpenSSL, curl, zlib, brotli, libxml2 and more

## Strategy

- Move to the current stable series of nginx instead of patching an old
  one
- Pick up fixes of Alpine published after the base image while building
- Leave the website and its configuration as they are

## Dockerfile

- The runtime stage starts from `nginxinc/nginx-unprivileged:1.30-alpine`,
  the stable series on Alpine 3.24
- `apk upgrade --no-cache` runs as root before the files are copied; the
  server still runs as the user 1000 on port 8080
- The build stage with Node.js and Vite stays as it was

## Result

- The image builds; the scan of the next image shows the findings left

## Open Points

- New vulnerabilities appear over time; the image has to be rebuilt to
  pick up their fixes
