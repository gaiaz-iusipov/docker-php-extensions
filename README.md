# docker-php-extensions

[![GitHub](https://img.shields.io/github/license/gaiaz-iusipov/docker-php-extensions.svg)](https://github.com/gaiaz-iusipov/docker-php-extensions#license)
[![Docker Automated build](https://img.shields.io/docker/cloud/automated/gaiaz/php-extensions.svg)](https://cloud.docker.com/repository/docker/gaiaz/php-extensions)
[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/gaiaz/php-extensions.svg)](https://cloud.docker.com/repository/docker/gaiaz/php-extensions)
[![Docker Pulls](https://img.shields.io/docker/pulls/gaiaz/php-extensions.svg)](https://hub.docker.com/r/gaiaz/php-extensions/)

:whale: A [Docker](https://www.docker.com/) image that contains prebuilt [PHP](https://hub.docker.com/_/php/) extensions.

## Extensions

- [APCu](https://pecl.php.net/package/APCu)
- GD
- Intl
- OPcache
- PDO_MYSQL
- PDO_PGSQL
- [redis](https://pecl.php.net/package/redis)
- [Xdebug](https://xdebug.org/)
- Zip

## Usage

```Dockerfile
FROM php:7.4-cli-alpine

COPY --from=gaiaz/php-extensions:7.4-alpine \
    /apcu.so \
    /gd.so \
    /intl.so \
    /opcache.so \
    /pdo_mysql.so \
    /pdo_pgsql.so \
    /redis.so \
    /xdebug.so \
    /zip.so \
    /usr/local/lib/php/extensions/no-debug-non-zts-20190902/

RUN docker-php-ext-enable \
        apcu \
        opcache \
        pdo_mysql \
        redis \
        xdebug

RUN set -xe \
    && apk add --quiet --no-cache \
        libpng \
    && docker-php-ext-enable \
        gd

RUN set -xe \
    && apk add --quiet --no-cache \
        icu \
    && docker-php-ext-enable \
        intl

RUN set -xe \
    && apk add --quiet --no-cache \
        libpq \
    && docker-php-ext-enable \
        pdo_pgsql

RUN set -xe \
    && apk add --quiet --no-cache \
        libzip \
    && docker-php-ext-enable \
        zip

RUN php -m
```

## License

[MIT](http://opensource.org/licenses/MIT) © [Gaiaz Iusipov](https://github.com/gaiaz-iusipov)
