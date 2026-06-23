# Docker Collection

A collection of Docker Compose configurations for a self-hosted homelab. All services share an external Docker network called `homelab` and are routed via Traefik as the reverse proxy.

## Setup

Create the shared network before starting any service:

```bash
docker network create homelab
```

Then start individual services:

```bash
cd <service-folder>
cp .env .env  # fill in the variables
docker compose up -d
```

## Services

| Service | Description | Port / URL |
|---|---|---|
| [Traefik](traefik/) | Reverse proxy & TLS termination | `:80`, `:443` |
| [Immich](immich/) | Photo & video management (self-hosted Google Photos) | `https://images.cloud.lan` |
| [Jellyfin](jellyfin/) | Media server for movies & TV shows | `:8096` |
| [CherryMusic](cherrymusic/) | Music streaming server | `:CHERRYMUSIC_PORT` |
| [Homarr](Homarr/) | Homelab dashboard with Docker integration | `:7575` |
| [File Browser](filebrowser/) | Web-based file manager | `:8095` |
| [Nextcloud AIO](nextcloud-all-in-one/) | File sync & sharing (bundled DB) | `:8080` |
| [Nextcloud](nextcloud/) | File sync & sharing (external DB) | `:8080` |
| [MariaDB](mariadb/) | Standalone MariaDB database | internal |
| [Nginx](nginx/) | Static web server / custom reverse proxy | `:NGINX_HTTP_PORT` |
| [Nginx Proxy Manager](nginx-proxy-manager/) | GUI-based reverse proxy & SSL manager | `:81` (admin) |
| [Dash.](dashdot/) | Server system stats dashboard | `:3000` |

## Network Architecture

```
Internet / LAN
      │
   Traefik  (:80 → :443)
      │
  homelab network
  ┌───┴────────────────────────┐
  │  immich  jellyfin  homarr  │
  │  filebrowser  cherrymusic  │
  │  nextcloud  nginx  ...     │
  └────────────────────────────┘
```

All services connect to the `homelab` external network. Traefik picks up routing rules from Docker labels automatically.

## Directory Structure

```
docker-collection/
├── traefik/             # Reverse proxy (start this first)
├── immich/              # Photo management
├── jellyfin/            # Media server
├── cherrymusic/         # Music streaming
├── Homarr/              # Homelab dashboard
├── filebrowser/         # File manager
├── nextcloud-all-in-one/# Nextcloud + MariaDB bundled
├── nextcloud/           # Nextcloud (standalone)
├── mariadb/             # Shared MariaDB instance
├── nginx/               # Nginx web server
├── nginx-proxy-manager/ # GUI reverse proxy
└── dashdot/             # System stats dashboard
```

## Notes

- **Start Traefik first** — other services depend on it for routing.
- Each service folder contains its own `docker-compose.yml`, `.env`, and `README.md`.
- Fill in all `.env` variables before starting a service.
- Never commit `.env` files with real credentials to version control.
