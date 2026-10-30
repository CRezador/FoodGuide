FROM php:8.4-fpm-alpine

ARG UID=1000
ARG GID=1000

RUN apk add --no-cache icu-dev libzip-dev oniguruma-dev linux-headers $PHPIZE_DEPS \
    && docker-php-ext-install pdo_mysql bcmath intl zip pcntl opcache \
    && apk del $PHPIZE_DEPS linux-headers \
    && apk add --no-cache icu-libs libzip

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
COPY docker/php/php.ini /usr/local/etc/php/conf.d/food.ini

# Roda o PHP-FPM com o mesmo UID/GID do seu usuário, para não brigar com permissões no volume.
RUN apk add --no-cache shadow \
    && usermod -u ${UID} www-data \
    && groupmod -g ${GID} www-data

WORKDIR /var/www/html
USER www-data
