# Dash.

A modern, minimal server dashboard that displays live system stats (CPU, RAM, storage, network).

## Requirements

- Docker & Docker Compose
- External Docker network `homelab` must exist:
  ```bash
  docker network create homelab
  ```
- Runs in `privileged` mode to access host system metrics

## Configuration

Fill in the `.env` file before starting:

| Variable | Description | Example |
|---|---|---|
| `DASHDOT_APPDATA_PATH` | Host path for Dash. app data | `/opt/dashdot` |
| `DASHDOT_ENABLE_CPU_TEMPS` | Show CPU temperature (`true`/`false`) | `true` |
| `DASHDOT_SPEED_TEST_FROM_PATH` | Path used for disk speed test | `/` |
| `DASHDOT_SHOW_DASH_VERSION` | Show Dash. version in UI (`true`/`false`) | `true` |

## Usage

```bash
docker compose up -d
```

## Ports

| Port | Purpose |
|---|---|
| `3000` | Web UI |
| `3001` | Internal server |
| `3002` | API docs |

## Notes

- The host root `/` is mounted as `/mnt/host` (read-only) so Dash. can read system information.
- `privileged: true` is required for hardware sensor access.
