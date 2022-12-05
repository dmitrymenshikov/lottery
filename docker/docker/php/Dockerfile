FROM php:7.4-fpm-alpine

# Install required librairies
RUN apk update && apk add autoconf g++ make git \
    unzip \
    supervisor \
    postgresql-dev \
    libzip-dev \
    libpng-dev \
    icu-dev \
    libxml2-dev \
    rabbitmq-c-dev \
    libmemcached-dev \
    memcached

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/bin --filename=composer --quiet
RUN composer --version

# Install PHP extensions
RUN docker-php-ext-install pdo pdo_mysql pdo_pgsql zip gd intl bcmath soap sockets opcache

RUN pecl install http://pecl.php.net/get/amqp-1.10.2.tgz
RUN docker-php-ext-enable amqp

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN pecl install memcached
RUN docker-php-ext-enable memcached.so

RUN rm -rf /tmp/pear

COPY ./supervisor.conf /etc/supervisor/conf.d/supervisor.conf

# Import custom php.ini
COPY ./php.ini /usr/local/etc/php/conf.d

WORKDIR /var/www

COPY ./entrypoint.sh /usr/local/bin/docker-php-entrypoint
RUN chmod +x /usr/local/bin/docker-php-entrypoint
