# Fider customer feedback

Customer feedback and feature voting application with a Go backend and a React/TypeScript frontend.

This repository is a separately imported source snapshot of [fider](https://github.com/getfider/fider). Upstream contributors created the imported application; local work is recorded in the commits after the source import. See [Upstream source](UPSTREAM_SOURCE.md) for attribution.

## Read the history accurately

The local history starts with a source import on November 3, 2026. The parent year directory and the “Original project year” field do not establish a historical contribution date or a release tag. The imported upstream commit and release are not recorded in the available provenance file.

## Navigate the source

| Path | Responsibility |
| --- | --- |
| [app/handlers/](app/handlers/) | HTTP handlers |
| [app/services/](app/services/) | Application services |
| [public/pages/](public/pages/) | Frontend pages |
| [migrations/](migrations/) | SQL migrations |
| [GUIDELINES.md](GUIDELINES.md) | Backend and UI conventions |

## Prepare development

Read `go.mod`, `package.json`, `Makefile`, and `.example.env`. Create local configuration from `.example.env` and configure the database before migrations or tests. The test target uses `.test.env`; ensure it points to a disposable test database.

The commands below are existing project entry points, to run after their toolchain, dependency, and service prerequisites are ready:

```sh
make build
make lint
make test
```

## Validation scope

The documentation change was checked for local link targets, manifest command references, and whitespace errors. Application builds, dependency installation, and service-backed test suites were not run for this documentation change. These instructions do not establish deployment readiness.

## Preserve upstream licensing

Read [LICENSE](LICENSE) and the notices in individual source files. This documentation does not change the license of imported code.

## Install and run locally

Match the Go/Node.js versions in the manifests. Install JavaScript dependencies and create local configuration:

```sh
npm ci
cp .example.env .env
```

Edit `.env`: `BASE_URL` must match the local application address, `DATABASE_URL` must point to your development PostgreSQL database, and `JWT_SECRET` must be replaced. The Compose file provides a PostgreSQL service on port 5555 and a local SMTP service:

```sh
docker compose up -d pgdev smtp
make build
make migrate
make run
```

Create the database named in `DATABASE_URL` before migrations. The `make run` and migration targets need the `godotenv` executable referenced in the Makefile. Configure `.test.env` against the separate disposable test service before `make test`.

## Plan a deployment

Review `Dockerfile` and the environment template for the database, public base URL, JWT signing secret, and mail delivery settings. The included Compose database passwords are development values. Persist PostgreSQL data and arrange database backups before exposing the application.
