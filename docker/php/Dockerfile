FROM php:8.3-fpm-alpine

# 必要なパッケージのインストール
RUN apk add --no-cache \
    git \
    curl \
    libpng-dev \
    libjpeg-turbo-dev \
    freetype-dev \
    libzip-dev \
    icu-dev \
    oniguruma-dev \
    postgresql-dev \
    bash \
    linux-headers \
    $PHPIZE_DEPS

# PHP拡張のインストール
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
        pdo \
        pdo_mysql \
        pdo_pgsql \
        pgsql \
        mysqli \
        gd \
        intl \
        zip \
        opcache \
        mbstring \
        exif \
        bcmath

# Xdebugのインストール
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Composerのインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 作業ディレクトリ
WORKDIR /var/www/html

# ログディレクトリ作成
RUN mkdir -p /var/log/php && chown www-data:www-data /var/log/php

# ユーザー権限設定
RUN chown -R www-data:www-data /var/www/html
