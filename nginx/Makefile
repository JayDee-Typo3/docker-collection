# === Konfiguration ===
PROJECT_DIR=
PHP_CONTAINER=php-fpm
NGINX_CONTAINER=

help:
	@echo "Verfügbare Make-Befehle:"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' Makefile | awk 'BEGIN {FS = ":.*?## "}; {printf "  \033[36m%-20s\033[0m %s\n", $$1, $$2}'

# === Container starten/stoppen ===
up: ## Start container
	docker compose up -d

down: ## Stop container
	docker compose down

restart: ## Restart container
	docker compose down && docker compose up -d

# === Symfony Setup ===
symfony-new: ## Generate new symfony app
	docker exec -it $(PHP_CONTAINER) symfony new . --webapp

composer-install: ## Execute composer install
	docker exec -it $(PHP_CONTAINER) composer install

cache-clear: ## Clear symfony cache
	docker exec -it $(PHP_CONTAINER) php bin/console cache:clear

# === Rechte setzen ===
permissions: ## Set user permissions in symfony project directory
	sudo chown -R $$(id -u):$$(id -g) $(PROJECT_DIR)
	sudo chmod -R 775 $(PROJECT_DIR)/var

# === Datenbank ===
db-migrate: ## Database migration
	docker exec -it $(PHP_CONTAINER) php bin/console doctrine:migrations:migrate

db-update: ## update schema automatecally
	docker exec -it $(PHP_CONTAINER) php bin/console doctrine:schema:update --force

# === Logs anschauen ===
logs: ## Show all server logs
	docker compose logs -f

nginx-logs: ## Show logs of container
	docker logs -f $(NGINX_CONTAINER)

php-logs: ## Show php logs
	docker logs -f $(PHP_CONTAINER)

# === Debug ===
bash: ## Open bash in php container
	docker exec -it $(PHP_CONTAINER) sh

nginx-bash: ## Open bash in nginx container
	docker exec -it $(NGINX_CONTAINER) sh

symfony-new: ## Generate new Symfony app
docker exec -it $(PHP_CONTAINER) composer create-project symfony/website-skeleton .
