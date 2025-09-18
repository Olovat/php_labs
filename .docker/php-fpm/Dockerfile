FROM php:8.4-cli-alpine AS builder
#попытка в multi-stage сборку, ибо 900 мегабайт для простого пхп много чёт
RUN apk add --no-cache curl git libzip-dev zip unzip autoconf make gcc g++ musl-dev \
 && docker-php-ext-configure zip \
 && docker-php-ext-install zip

WORKDIR /app
COPY composer.json composer.lock* /app/
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
 && composer install --no-dev --prefer-dist --no-interaction --no-progress --optimize-autoloader

COPY . /app

FROM php:8.4-fpm-alpine

RUN apk add --no-cache libzip

WORKDIR /var/www/html
COPY --from=builder /app /var/www/html

RUN chown -R www-data:www-data /var/www/html || true

EXPOSE 9000
CMD ["php-fpm"]