.PHONY: help
.DEFAULT_GOAL := help
$(VERBOSE).SILENT:

help: ## Print the help screen
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

TYPE ?= "docker"
benchmark: ## Runs the benchmark
	./benchmark.sh "$(TYPE)"

composer-install: ## Install Composer dependencies
	docker run --rm --interactive --tty --volume $(PWD):/app --user $(id -u):$(id -g) composer install --ignore-platform-reqs

composer-update: ## Update Composer dependencies
	docker run --rm --interactive --tty --volume $(PWD):/app --user $(id -u):$(id -g) composer update --ignore-platform-reqs

phpstan: ## Run PHPStan to perform static analysis
	docker-compose run --rm --no-deps benchmark-cli /bin/sh -c "cd /code && ./vendor/bin/phpstan analyse --level 8 src"

cs: ## Run PHP CodeSniffer to detect issues with coding style
	docker-compose run --no-deps --rm benchmark-cli /code/vendor/bin/phpcs --standard=/code/phpcs.xml

cs-fix: ## Run PHPCBF to automatically fix issues with coding style
	docker-compose run --no-deps --rm benchmark-cli /code/vendor/bin/phpcbf --standard=/code/phpcs.xml

qa: phpstan cs ## Test code quality
