# Nginx Proxy Manager

Web-based GUI for managing Nginx reverse proxy hosts, redirections, streams, and SSL certificates (including Let's Encrypt).

## Requirements

- Docker & Docker Compose
- Ports `80` and `443` available on the host
- External Docker network `homelab` must exist:
  ```bash
  docker network create homelab
  ```

## Configuration

The database credentials are configured directly in the `docker-compose.yml` environment section. The `.env` file can be used to override them if needed.

| Variable | Description |
|---|---|
| `DB_MYSQL_HOST` | MariaDB hostname (internal: `db`) |
| `DB_MYSQL_PORT` | MariaDB port (`3306`) |
| `DB_MYSQL_USER` | Database user |
| `DB_MYSQL_PASSWORD` | Database password |
| `DB_MYSQL_NAME` | Database name |

## Usage

```bash
docker compose up -d
```

The admin UI is available at `http://<host>:81`.

Default credentials on first login:
- **Email:** `admin@example.com`
- **Password:** `changeme`

Change these immediately after first login.

## Ports

| Port | Purpose |
|---|---|
| `80` | Public HTTP (proxied traffic) |
| `443` | Public HTTPS (proxied traffic) |
| `81` | Admin web UI |

## Data

| Path | Purpose |
|---|---|
| `./data` | Proxy configuration, user data |
| `./letsencrypt` | SSL certificates |

## Notes

- For a code-based reverse proxy with Docker label routing, see `../traefik`.
- SSL certificates are stored in `./letsencrypt` and auto-renewed by NPM.
