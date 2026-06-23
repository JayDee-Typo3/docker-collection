# Immich

Self-hosted photo and video management solution with machine learning features (face detection, smart search, CLIP embeddings).

## Requirements

- Docker & Docker Compose
- External Docker network `homelab` must exist:
  ```bash
  docker network create homelab
  ```
- Traefik running on the `homelab` network (for HTTPS routing)

## Configuration

Copy and fill in the `.env` file before starting:

```bash
cp .env .env.local  # optional — or edit .env directly
```

### Variables

| Variable | Description | Example |
|---|---|---|
| `IMMICH_VERSION` | Image tag — leave empty to use `release` | `v1.132.0` |
| `UPLOAD_LOCATION` | Host path for photos/videos | `/mnt/nas/immich/upload` |
| `DB_DATA_LOCATION` | Host path for Postgres data | `/opt/immich/postgres` |
| `DB_PASSWORD` | Postgres password | `changeme` |
| `DB_USERNAME` | Postgres user | `immich` |
| `DB_DATABASE_NAME` | Postgres database name | `immich` |

### Machine Learning (optional)

| Variable | Description | Recommended (Pi 4) |
|---|---|---|
| `MACHINE_LEARNING_ENABLED` | Set to `false` to disable ML entirely (saves ~1–2 GB RAM) | `true` |
| `MACHINE_LEARNING_WORKERS` | Number of concurrent ML workers | `1` |
| `MACHINE_LEARNING_WORKER_TIMEOUT` | Seconds before idle worker is stopped to free RAM | `120` |
| `MACHINE_LEARNING_MODEL_INTER_OP_THREADS` | CPU thread parallelism between ops | `1` |
| `MACHINE_LEARNING_MODEL_INTRA_OP_THREADS` | CPU threads within a single op | `2` |

> On low-RAM devices (e.g. Raspberry Pi 4), set `MACHINE_LEARNING_WORKERS=1` and `MACHINE_LEARNING_WORKER_TIMEOUT=120` at minimum. If memory is still too tight, set `MACHINE_LEARNING_ENABLED=false` to disable ML entirely.

## Services

| Container | Image | Purpose |
|---|---|---|
| `immich_server` | `ghcr.io/immich-app/immich-server` | Main application & API |
| `immich_machine_learning` | `ghcr.io/immich-app/immich-machine-learning` | Face detection, smart search |
| `immich_redis` | `valkey/valkey:9` | Cache & job queue |
| `immich_postgres` | `ghcr.io/immich-app/postgres` | Database |

## Usage

```bash
docker compose up -d
```

The service is available at **https://images.cloud.lan** (routed via Traefik).

## Networking

`immich-server` is attached to the external `homelab` network so Traefik can route traffic to it. Internal services (`redis`, `database`, `immich-machine-learning`) communicate over the default Compose network and are not exposed to the homelab network.

## Traefik

The following labels are configured on `immich-server`:

- **Host:** `images.cloud.lan`
- **Entrypoint:** `websecure` (HTTPS, port 443)
- **TLS:** enabled
- HTTP → HTTPS redirect is handled globally by Traefik

Make sure `images.cloud.lan` resolves to your Traefik host in your local DNS.
