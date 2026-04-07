# Qorstack Report — Self-Host

Get up and running in minutes with Docker Compose.

## Quick Start

```bash
git clone https://github.com/qorstack/qorstack-report-opensource.git
cd qorstack-report-opensource
cp .env.example .env
```

Edit `.env` with your values, then:

```bash
docker compose up -d
```

---

## Access

| Service       | URL                     | Default credentials                                 |
| ------------- | ----------------------- | --------------------------------------------------- |
| Web app       | <http://localhost:3000> | `ADMIN_EMAIL` / `ADMIN_PASSWORD` from `.env`        |
| API           | <http://localhost:8080> | —                                                   |
| MinIO console | <http://localhost:9001> | `MINIO_ACCESS_KEY` / `MINIO_SECRET_KEY` from `.env` |

---

## Connecting with a Database Client (DBeaver, TablePlus, etc.)

PostgreSQL is exposed on port **5432** of the host machine.

| Field    | Value                              |
| -------- | ---------------------------------- |
| Host     | `localhost`                        |
| Port     | `5432`                             |
| Database | `qorstack_report`                  |
| Username | `postgres`                         |
| Password | value of `DB_PASSWORD` in `.env`   |

> **Note:** If port 5432 is already in use on your machine, change the host port in `docker-compose.yml`:
> ```yaml
> ports:
>   - "5433:5432"   # connect on localhost:5433 instead
> ```

---

## Connecting to MinIO

MinIO S3 API is on port **9000**, the web console on **9001**.

| Field          | Value                                |
| -------------- | ------------------------------------ |
| Endpoint       | `http://localhost:9000`              |
| Access Key     | value of `MINIO_ACCESS_KEY` in `.env` |
| Secret Key     | value of `MINIO_SECRET_KEY` in `.env` |
| Console URL    | <http://localhost:9001>              |

---

## Gotenberg (PDF engine)

Gotenberg is an internal service — it is **not exposed** on any host port by design.
The backend container communicates with it directly over the Docker network.

To verify it is running:

```bash
docker compose ps gotenberg
docker compose logs gotenberg
```

To test it manually from inside the network:

```bash
docker compose exec backend curl -s http://qorstack-gotenberg:3000/health
```

---

## Using your own PostgreSQL or MinIO _(optional)_

If you already have a running PostgreSQL or MinIO, uncomment the relevant lines in `.env`:

```env
DB_HOST=your-postgres-host
DB_PORT=5432
DB_NAME=qorstack_report
DB_USER=postgres

MINIO_ENDPOINT=your-minio-host:9000
MINIO_USE_SSL=false
```

Then remove the corresponding service (`postgres` / `minio`) from `docker-compose.yml` so it is not started.

---

## Custom Fonts _(optional)_

Place `.ttf` or `.otf` files in the `fonts/` folder, then restart the backend:

```bash
docker compose restart backend
```

> Fonts are automatically synced to the Gotenberg container — no restart of Gotenberg is needed.

To download all Google Fonts at once:

```bash
bash download-google-fonts.sh
docker compose restart backend
```

---

## Updating

```bash
docker compose pull
docker compose up -d
```

---

## Source Code

This repository contains only the self-hosting configuration files.  
The full source code is open on GitHub: [qorstack/qorstack-report](https://github.com/qorstack/qorstack-report)

- Full documentation (Pro license, troubleshooting) → [README](https://github.com/qorstack/qorstack-report#readme)
- Bug reports and feature requests → [Issues](https://github.com/qorstack/qorstack-report/issues)
