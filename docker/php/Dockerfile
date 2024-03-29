FROM php:fpm-alpine

ADD ./docker/php/www.conf /usr/local/etc/php-fpm.d/www.conf
COPY ./docker/php/xdebug.ini /usr/local/etc/php/conf.d/
RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

RUN mkdir -p /var/www/html

RUN chown laravel:laravel /var/www/html

WORKDIR /var/www/html

RUN apk update \
 && apk --quiet add --no-cache $PHPIZE_DEPS \
    libzip-dev \
    libsodium-dev \
    curl-dev \
    openssl-dev \
    yarn \
    bash \
    npm \
    composer \
    freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev && \
  docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j$(nproc) gd && \
  apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev



RUN docker-php-ext-install -j$(nproc) pdo pdo_mysql zip exif pcntl bcmath sodium curl
RUN pecl install mongodb && docker-php-ext-enable mongodb && pecl install xdebug
