# CherryMusic

Self-hosted music streaming server with a web interface. Stream your music collection from anywhere via browser.

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
| `CHERRYMUSIC_CONFIG_FOLDER` | Host path for CherryMusic config files | `/opt/cherrymusic/config` |
| `CHERRYMUSIC_DATA_FOLDER` | Host path for CherryMusic data | `/opt/cherrymusic/data` |
| `CHERRYMUSIC_MUSIC_FOLER` | Host path to your music library (read-only) | `/mnt/nas/music` |
| `CHERRYMUSIC_PORT` | Host port to expose the web UI | `8090` |

## Usage

```bash
docker compose up -d
```

The web UI is available at `http://<host>:<CHERRYMUSIC_PORT>`.

## Ports

| Port | Purpose |
|---|---|
| `CHERRYMUSIC_PORT` → `8080` | Web interface |
| `8443` | HTTPS (if SSL enabled) |

## Notes

- On first start, set `CONFIG_DOWNLOAD_ENABLE=1` in the compose file to download the default config, then remove it.
- SSL can be enabled by uncommenting `SELF_SIGNED_CERT_CREATE` and `SERVER_SSL_ENABLED` in the compose file.
