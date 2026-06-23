# Nextcloud All-in-One

Self-hosted file sync and sharing platform. This setup bundles both Nextcloud and MariaDB in a single Compose stack — no external database needed.

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
| `MYSQL_HOST` | MariaDB hostname (internal, pre-set to `mariadb`) | `mariadb` |
| `MYSQL_ROOT_PASSWORD` | MariaDB root password | `changeme` |
| `MYSQL_USER` | Database user for Nextcloud | `nextcloud` |
| `MYSQL_PASSWORD` | Password for the database user | `changeme` |
| `MYSQL_DATABASE` | Database name | `nextcloud` |
| `MYSQL_VOLUME_PATH` | Host path for MariaDB data | `/opt/nextcloud/mariadb` |
| `NEXTCLOUD_ADMIN_USER` | Initial admin username | `admin` |
| `NEXTCLOUD_ADMIN_PASSWORD` | Initial admin password | `changeme` |
| `NEXTCLOUD_TRUSTED_DOMAIN` | Comma-separated list of trusted domains/IPs | `192.168.1.10,cloud.lan` |
| `NEXTCLOUD_CONFIG_PATH` | Host path for Nextcloud config | `/opt/nextcloud/config` |
| `NEXTCLOUD_PHOTOS_PATH` | Host path for photos (optional bind mount) | `/mnt/nas/photos` |
| `NEXTCLOUD_DATA_PATH` | Host path for Nextcloud HTML/data | `/opt/nextcloud/data` |

## Usage

```bash
docker compose up -d
```

The web UI is available at `http://<host>:8080`.

## Ports

| Port | Purpose |
|---|---|
| `8080` | Web interface |

## Notes

- MariaDB and Nextcloud communicate over the default Compose network — the database is not exposed externally.
- See `../nextcloud` for a setup that uses a standalone external MariaDB instance.
- `NEXTCLOUD_TRUSTED_DOMAIN` must include every IP or hostname used to access Nextcloud.
