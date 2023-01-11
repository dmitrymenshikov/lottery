FROM php:8.1-fpm-alpine

# Install required librairies
RUN apk update && apk add autoconf g++ make git \
    autoconf \
    libzip-dev \
    libpng-dev \
    icu-dev \
    libxml2-dev \
    rabbitmq-c-dev \
    build-base

RUN docker-php-ext-install pdo pdo_mysql zip gd intl bcmath soap sockets opcache

# Install composer
COPY --from=composer:2.1 /usr/bin/composer /usr/local/bin/composer
RUN composer --version

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

RUN pecl install amqp
RUN docker-php-ext-enable amqp

#RUN pecl install xdebug
#RUN docker-php-ext-enable xdebug

RUN rm -rf /tmp/pear

COPY ./supervisor.conf /etc/supervisor/conf.d/supervisor.conf

# Import custom php.ini
COPY ./php.ini /usr/local/etc/php/conf.d

WORKDIR /var/www

RUN export DOCKER_SCAN_SUGGEST=false