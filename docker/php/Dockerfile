FROM php:8.3-fpm

# Installation des dépendances système et extensions PHP
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libzip-dev \
    libicu-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo pdo_mysql zip intl opcache \
    && pecl install apcu \
    && docker-php-ext-enable apcu opcache

# Installation de Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Configuration de PHP
COPY docker/php/php.ini /usr/local/etc/php/conf.d/app.ini

# Configuration OPcache pour le CLI
RUN echo "opcache.enable_cli=1" >> /usr/local/etc/php/conf.d/opcache.ini

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]