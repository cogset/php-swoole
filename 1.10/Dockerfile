# PHP with Swoole
# Cogset Open Source Group

FROM php:7.3-cli

MAINTAINER Cogset <cogset@funcuter.org>

ENV PHP_SWOOLE_VERSION 1.10.6

RUN set -ex; \
        \
        apt-get update; \
        apt-get install -y \
            libsodium-dev \
            libzip-dev \
            libssl-dev \
            zlib1g-dev \
            libpng-dev \
            libjpeg-dev \
            libfreetype6-dev \
            openssh-client \
            git \
            procps \
            cron \
            supervisor \
            zip \
            unzip \
            --no-install-recommends; \
        rm -r /var/lib/apt/lists/*

RUN set -ex; \
        \
        docker-php-ext-install \
            bcmath \
            calendar \
            exif \
            pcntl \
            shmop \
            sockets \
            sodium \
            zip \
            sysvmsg \
            sysvsem \
            sysvshm \
            pdo_mysql

RUN set -ex; \
        \
        docker-php-source extract; \
        cd /usr/src/php/ext/gd; \
        phpize; \
        ./configure \
            --with-freetype-dir=/usr/include/freetype2 \
            --with-jpeg-dir=/usr/include; \
        make && make install; \
        docker-php-ext-enable gd; \
        cd /

RUN set -ex; \
        \
        curl -L https://github.com/swoole/swoole-src/archive/v$PHP_SWOOLE_VERSION.tar.gz -o php-swoole.tar.gz; \
        mkdir -p /usr/src/php-swoole; \
        tar -xzf php-swoole.tar.gz -C /usr/src/php-swoole --strip-components=1; \
        rm -rf php-swoole.tar.gz; \
        cd /usr/src/php-swoole; \
        phpize; \
        ./configure \
            --enable-openssl \
            --enable-mysqlnd; \
        make && make install; \
        docker-php-ext-enable swoole; \
        cd /

EXPOSE 8080