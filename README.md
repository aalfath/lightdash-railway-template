[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new/template/lightdash-1?utm_medium=integration&utm_source=button&utm_campaign=lightdash)

# Lightdash on Railway

One-click [Lightdash](https://github.com/lightdash/lightdash) (open-source BI on top of your dbt project) for Railway, with the services upstream runs in production.

**Version:** Lightdash `3.341.0` (`lightdash/lightdash:3.341.0@sha256:8242b5ff0f9522f6c31c0b63184325d4643be32ee7b6d32599716d1f2686f49a`), Browserless `v2.49.0` (`ghcr.io/browserless/chromium:v2.49.0@sha256:a0c024087a2625704ffe3644ea2d886324b57ab1cc9124fb15eacc6597429ca1`, the version Lightdash pins upstream).

## What it deploys

| Service | Source | What it does |
|---|---|---|
| `Lightdash` | `lightdash/` in this repo | Web app and API, the only public service. Runs the database migrations on boot. Health check `/api/v1/health`. |
| `Scheduler` | `scheduler/` in this repo | Background worker (`node dist/scheduler.js`): scheduled deliveries, CSV/XLSX/image exports, validation. Health check `/api/v1/health`. |
| `Browserless` | `browserless/` in this repo | Headless Chromium that renders chart and dashboard images. Private network only. |
| `Postgres` | Railway PostgreSQL 18 (with a volume) | Lightdash's metadata: users, projects, charts, dashboards, schedules. |
| `Bucket` | Railway Bucket (S3) | Query results, exports and rendered images. Downloads use presigned links. |

## Variables

All variables are set by the template; the deployer is asked for nothing.

- `LIGHTDASH_SECRET`: generated (`${{secret(64)}}`) on `Lightdash`; `Scheduler` references it. Encrypts warehouse credentials, so never change it after the first deploy.
- `SITE_URL`: `https://${{Lightdash.RAILWAY_PUBLIC_DOMAIN}}`. Change it if you add a custom domain.
- `PG*`: references to the `Postgres` service. `S3_*`: references to the `Bucket`.
- `HEADLESS_BROWSER_HOST` / `HEADLESS_BROWSER_PORT`: reference the `Browserless` service.
- Fixed settings (`SECURE_COOKIES=true`, `TRUST_PROXY=true`, `SCHEDULER_ENABLED=false` on the web service, port 8080) are in the Dockerfiles; any of them can be overridden with a Railway variable of the same name.

Optional features you can add later as variables on both `Lightdash` and `Scheduler`: SMTP (`EMAIL_SMTP_*`, for invites and email deliveries), Slack (`SLACK_*`), Google SSO (`AUTH_GOOGLE_*`), AI copilot keys. See the [Lightdash environment variables](https://docs.lightdash.com/self-host/customize-deployment/environment-variables).

## After deploy

1. Open the `Lightdash` public URL and register. The first account creates the organization and becomes its admin; after that, new users need an invite.
2. Connect your warehouse and dbt project (GitHub, GitLab, Bitbucket, Azure DevOps, dbt Cloud, or `lightdash deploy` from the CLI).

## Updates

Every service builds from this repo. When we bump the pinned versions on `main`, Railway shows an "update available" notice on your project, and you choose when to apply it. Read [CHANGELOG.md](CHANGELOG.md) first: breaking changes are listed at the top of each entry. Lightdash migrates its database on boot.

## Local test

`test/compose.yml` runs the same Dockerfiles locally, with MinIO standing in for the Railway Bucket (local test only, not used on Railway).
