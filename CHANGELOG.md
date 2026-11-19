# Changelog

## Unreleased

- The image is based on `nginx-unprivileged:1.30-alpine`, the stable
  series, and upgrades its Alpine packages while building: no high or
  critical findings with a fix
- `deploy.yaml` can be started by hand with Run workflow, to deploy
  `main` again
- A push to `main` builds the image and deploys it to `dev`: the
  workflow `deploy.yaml` calls the workflow `build` of `cars-deployment`
- Move the repository to the GitHub organization `cars-world` and
  rename the package to `@cars-world/cars-origin-client`
- Ask `api/hello` relative to the page, so the page works under any
  path, e.g. behind a proxy that strips a prefix
- Build the image into the registry `crcars`, tagged with the version
- Name the stages `dev`, `test` and `prod` in the README
- Add the hello world after the setup of `cars-client`: the static page, its `showHello()` and the nginx container that forwards `/api` to `MASTER_URL` and answers `GET /health`
