# Traefik

Modern reverse proxy and load balancer with automatic Docker service discovery. Routes traffic to services via Docker labels and supports TLS termination.

## Requirements

- Docker & Docker Compose
- Ports `80`, `443`, and `8080` available on the host
- External Docker network `homelab` must exist:
  ```bash
  docker network create homelab
  ```
- TLS certificates available at `CERTIFICATE_FOLDER_PATH`
- A dynamic config folder with at least a `tls.yaml` file

## Configuration

Fill in the `.env` file before starting:

| Variable | Description | Example |
|---|---|---|
| `DOCKER_SOCKET_PATH` | Path to Docker socket | `/var/run/docker.sock` |
| `CERTIFICATE_FOLDER_PATH` | Host path containing TLS certificates | `/opt/traefik/certs` |
| `DYNAMIC_CONFIG_FOLDER_PATH` | Host path for dynamic config files (e.g. `tls.yaml`) | `/opt/traefik/dynamic` |
| `TLS_FILE_PATH` | Path to the TLS config file *inside the container* | `/dynamic/tls.yaml` |
| `TRAEFIK_DASHBOARD_LABEL_HOST` | Hostname for the Traefik dashboard | `traefik.cloud.lan` |
| `TRAEFIK_BASIC_AUTH` | Basic auth string for dashboard (htpasswd format) | *(generate below)* |
| `TRAEFIK_WHOAMI_LABEL_HOST` | Hostname for the whoami test service | `whoami.cloud.lan` |

Generate a basic auth string:
```bash
echo $(htpasswd -nB admin) | sed -e 's/\$/\$\$/g'
```

## Usage

```bash
docker compose up -d
```

## Ports

| Port | Purpose |
|---|---|
| `80` | HTTP (auto-redirects to HTTPS) |
| `443` | HTTPS |
| `8080` | Traefik internal (not exposed externally in this config) |

## Adding Services

To route a service through Traefik, add it to the `homelab` network and attach these labels:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.services.<name>.loadbalancer.server.port=<internal-port>"
  - "traefik.http.routers.<name>.rule=Host(`<hostname>`)"
  - "traefik.http.routers.<name>.entrypoints=websecure"
  - "traefik.http.routers.<name>.tls=true"
  - "traefik.http.routers.<name>.service=<name>"
```

## Notes

- `exposedbydefault=false` means services must explicitly opt in via `traefik.enable=true`.
- HTTP → HTTPS redirect is configured globally; individual services only need the `websecure` entrypoint.
- The Docker socket is mounted read-only for security.
