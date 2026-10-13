### Install composer
FROM composer:latest AS composer

ENV APP_ENV=production
ENV COMPOSER_ALLOW_SUPERUSER=1

COPY . /src
WORKDIR /src

RUN ls -alh

RUN composer install --no-interaction --no-dev --prefer-dist --ignore-platform-reqs
RUN composer dump-autoload --optimize

# Build assets in an image that has PHP + Node
FROM php:8.5-cli AS assets

RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates curl gnupg git unzip libzip-dev \
  && docker-php-ext-install zip \
  && mkdir -p /etc/apt/keyrings \
  && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key \
     | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
  && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_24.x nodistro main" \
     > /etc/apt/sources.list.d/nodesource.list \
  && apt-get update \
  && apt-get install -y --no-install-recommends nodejs \
  && node -v && npm -v \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /assets
COPY --from=composer /src /assets

# npm ci preferred in CI/builds
RUN npm ci --no-audit --no-fund
# This will call "php artisan ..." via Wayfinder and works because php is available
RUN npm run build
RUN rm -rf node_modules

### build app container
FROM harbor.k8s.rapide.software/rapide-base-images/alpine-nginx-php-85
WORKDIR /app
USER 1000

COPY --chown=1000:1000  .kubernetes /config
COPY --chown=1000:1000  --from=assets /assets /app

ENV PHP_BOOT_SCRIPTS=/config/provision.sh

# set requirement privileges
RUN chown -R 1000:1000 storage && \
    touch storage/logs/laravel.log && \
	chmod -R 777 storage && \
	chown -R 1000:1000 bootstrap/cache && \
	chmod -R ug+rwx    bootstrap/cache && \
	chgrp -R 1000      bootstrap/cache && \
	chmod +x artisan && \
	chmod +x /config/provision.sh
