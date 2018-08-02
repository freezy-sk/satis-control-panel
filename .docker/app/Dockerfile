FROM php:7.2-fpm as base

# Install PHP extensions
RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install zip \
    && rm -rf /var/www/html

WORKDIR /var/www

# Build
# ==============================================================================
FROM base as build

# Composer setup
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_NO_INTERACTION 1

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
COPY composer.* ./

# Install composer packages
RUN composer install --no-dev --no-scripts --no-autoloader

COPY . .

RUN composer dump-autoload --optimize

# Production
# ==============================================================================
FROM base as production

# Set Laravel to log to error log
ENV APP_LOG errorlog

COPY . .
COPY --from=build /var/www/vendor ./vendor
