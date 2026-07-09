# ===========================
# Builder Stage
# ===========================
FROM php:8.2-fpm-alpine AS builder
# addd comment for tracking in status
RUN apk add --no-cache \
    $PHPIZE_DEPS \
    icu-dev \
    libzip-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    freetype-dev \
    oniguruma-dev \
    libxml2-dev \
    curl \
    git \
    unzip

RUN docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg

RUN docker-php-ext-install -j$(nproc) \
    mysqli \
    pdo_mysql \
    bcmath \
    exif \
    intl \
    gd \
    zip \
    opcache

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# ===========================
# Runtime Stage
# ===========================
FROM php:8.2-fpm-alpine

RUN apk add --no-cache \
    icu \
    libzip \
    libpng \
    libjpeg-turbo \
    freetype

COPY --from=builder /usr/local/lib/php/extensions /usr/local/lib/php/extensions
COPY --from=builder /usr/local/etc/php/conf.d /usr/local/etc/php/conf.d
COPY --from=builder /usr/bin/composer /usr/bin/composer

COPY php.ini /usr/local/etc/php/conf.d/custom.ini
COPY www.conf /usr/local/etc/php-fpm.d/www.conf

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
