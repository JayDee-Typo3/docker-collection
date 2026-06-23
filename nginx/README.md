# Nginx

Lightweight Nginx web server running on Alpine Linux. Used as a static file server or reverse proxy with a custom config.

## Requirements

- Docker & Docker Compose
- External Docker network `homelab` must exist:
  ```bash
  docker network create homelab
  ```
- A valid Nginx config file at the path specified by `NGINX_DEFAULT_CONFIG_PATH`

## Configuration

Fill in the `.env` file before starting:

| Variable | Description | Default |
|---|---|---|
| `NGINX_CONTAINER_NAME` | Name of the container | `nginx` |
| `NGINX_HTTP_PORT` | Host port for HTTP | `80` |
| `NGINX_DEFAULT_CONFIG_PATH` | Host path to the `nginx.conf` file | `./config/default.conf` |

## Usage

```bash
docker compose up -d
```

The server is available at `http://<host>:<NGINX_HTTP_PORT>`.

## Notes

- The config file is mounted read-only. Edit it on the host and restart the container to apply changes.
- For a GUI-based reverse proxy setup, see `../nginx-proxy-manager`.
