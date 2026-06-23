# File Browser

Web-based file manager that lets you browse, upload, download, and manage files on your server through a clean UI.

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
| `FILEBROWSER_CONTAINER_NAME` | Name of the container | `filebrowser` |
| `FILEBROWSER_MOUNT_ROOT_PATH` | Host path to expose as the root of the file browser | `/mnt/nas` |
| `FILEBROWSER_DATABASE_ROOT_PATH` | Host path where `filebrowser.db` is stored | `/opt/filebrowser` |
| `FILEBROWSER_SETTINGS_ROOT_PATH` | Host path for the settings/config directory | `/opt/filebrowser/config` |

## Usage

```bash
docker compose up -d
```

The web UI is available at `http://<host>:8095`.

Default credentials on first start: **admin / admin** — change immediately after login.

## Ports

| Port | Purpose |
|---|---|
| `8095` | Web interface |
