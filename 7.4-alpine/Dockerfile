FROM php:7.4-cli-alpine as builder

ENV BUILD_DEPS \
    autoconf \
    g++ \
    make

RUN apk add --quiet --no-cache \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd \
        --with-jpeg=/usr/include/ \
        --with-freetype=/usr/include/ \
    && docker-php-ext-install \
        gd > /dev/null

RUN apk add --quiet --no-cache \
        icu-dev \
    && docker-php-ext-install \
        intl > /dev/null

RUN docker-php-ext-install \
        opcache > /dev/null

RUN docker-php-ext-install \
        pdo_mysql > /dev/null

RUN apk add --quiet --no-cache \
        postgresql-dev \
    && docker-php-ext-install \
        pdo_pgsql > /dev/null

RUN apk add --quiet --no-cache \
        libzip-dev \
    && docker-php-ext-install \
        zip > /dev/null

RUN apk add --quiet --no-cache --virtual .build-deps $BUILD_DEPS \
    && pecl -q install \
        apcu \
        redis \
        xdebug \
    && apk del .build-deps

FROM scratch

LABEL maintainer="Gaiaz Iusipov <g.iusipov@gmail.com>"

COPY --from=builder \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/apcu.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/gd.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/intl.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/opcache.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/pdo_mysql.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/pdo_pgsql.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/redis.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/xdebug.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/zip.so \
    /
