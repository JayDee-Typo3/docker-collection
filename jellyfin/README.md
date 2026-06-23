# Jellyfin

Open-source media server for streaming movies, TV shows, music, and more. No subscription required.

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
| `JELLYFIN_CONFIG_FOLDER_PATH` | Host path for Jellyfin configuration | `/opt/jellyfin/config` |
| `JELLYFIN_CACHE_FOLDER_PATH` | Host path for Jellyfin cache/transcodes | `/opt/jellyfin/cache` |
| `JELLYFIN_MOVIES_FOLDER_SOURCE` | Host path to your movies library (read-only) | `/mnt/nas/movies` |
| `JELLYFIN_TVSHOWS_FOLDER_SOURCE` | Host path to your TV shows library (read-only) | `/mnt/nas/tvshows` |
| `JELLYFIN_FONTS_FOLDER_SOURCE` | Host path to custom fonts (optional) | `/opt/jellyfin/fonts` |
| `JELLYFIN_PUBLISHED_SERVER_URL` | Public URL Jellyfin advertises to clients | `http://jellyfin.lan` |

## Usage

```bash
docker compose up -d
```

The web UI is available at `http://<host>:8096`.

## Ports

| Port | Purpose |
|---|---|
| `8081` | Alternate HTTP port (mapped to internal 80) |
| `8096` | Main web UI & API |
| `8920` | HTTPS web UI |

## Notes

- The container runs as user `1000:0`. Make sure your media folders are readable by UID 1000.
- Media paths are mounted read-only to prevent accidental modification.
- For hardware transcoding on supported devices, see the [Jellyfin hardware acceleration docs](https://jellyfin.org/docs/general/administration/hardware-acceleration/).
