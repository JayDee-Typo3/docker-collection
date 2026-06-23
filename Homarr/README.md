# Homarr

A simple, yet powerful homelab dashboard. Integrates with Docker to auto-discover running services and displays them with status indicators.

## Requirements

- Docker & Docker Compose
- External Docker network `homelab` must exist:
  ```bash
  docker network create homelab
  ```

## Configuration

Fill in the `.env` file before starting:

| Variable | Description | Example |
|---|---|---|
| `HOMARR_DOCKER_SOCKET_PATH` | Path to Docker socket for container integration | `/var/run/docker.sock` |
| `HOMARR_APPDATA_PATH` | Host path for Homarr app data | `/opt/homarr` |
| `HOMARR_SECRET_ENCRYPTION_KEY` | 64-character hex key for encrypting secrets | *(generate with `openssl rand -hex 32`)* |
| `HOMARR_PORT` | Host port (currently unused in compose — port is hardcoded to 7575) | `7575` |

## Usage

```bash
docker compose up -d
```

The dashboard is available at `http://<host>:7575`.

## Ports

| Port | Purpose |
|---|---|
| `7575` | Web interface |

## Notes

- The Docker socket mount (`/var/run/docker.sock`) is optional but enables automatic service discovery and status monitoring.
- Generate the encryption key with: `openssl rand -hex 32`
