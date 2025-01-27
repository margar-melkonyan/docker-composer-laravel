FROM php:8.3-apache
# FROM php:8.0-apache

RUN apt-get update && apt-get upgrade -y \
    && apt-get install apt-utils -y \
    #
    #    устанавливаем необходимые пакеты
    && apt-get install git zip vim libzip-dev libpng-dev libgmp-dev libffi-dev libssl-dev libpq-dev -y \
    #
    #    Включаем необходимые расширения
    && docker-php-ext-install -j$(nproc) sockets zip gmp pcntl bcmath ffi \
    #
    #    Расшерения через pecl ставятся так, то в php 8 pecl сейчас отсутствует, так что строки закоментированы
    && PHP_OPENSSL=yes pecl install ev \
    && docker-php-ext-enable ev \
    # xdebug
    # && pecl install -o -f xdebug-3.3.1 \
    # && docker-php-ext-enable xdebug \
    # && docker-php-ext-install gd \
    # && docker-php-ext-install exif \
    #
    #    Add Redis extension
    && pecl install redis \
    && docker-php-ext-enable redis \
    #
    #    Чистим временные файлы
    && docker-php-source delete \
    && apt-get autoremove --purge -y && apt-get autoclean -y && apt-get clean -y

RUN docker-php-ext-install \
    pdo_mysql \
    pdo_pgsql \
    && a2enmod \
    rewrite

RUN docker-php-ext-enable pdo_mysql pdo_pgsql

# COPY ./conf.d /usr/locale/etc/php/conf.d
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www/

RUN rm -r html/

ENV APACHE_RUN_USER=#1000

EXPOSE 80
