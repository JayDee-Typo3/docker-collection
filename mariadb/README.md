# MariaDB

Standalone MariaDB database instance. Used as a shared database backend for services like Nextcloud.

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
| `MYSQL_HOST` | Hostname (pre-set to `mariadb`) | `mariadb` |
| `MYSQL_ROOT_PASSWORD` | Root password for MariaDB | `changeme` |
| `MYSQL_USER` | Application database user | `nextcloud` |
| `MYSQL_PASSWORD` | Password for the application user | `changeme` |
| `MYSQL_DATABASE` | Name of the default database to create | `nextcloud` |
| `MYSQL_DATA_PATH` | Host path for database data files | `/opt/mariadb` |

## Usage

```bash
docker compose up -d
```

## Notes

- Data is stored at `${MYSQL_DATA_PATH}/db` on the host.
- This instance is not directly exposed to the homelab network in the current compose — connect dependent services via the default Compose network or add the `homelab` network if cross-stack access is needed.
- For services in other Compose stacks to reach this database, they must share a common network.
