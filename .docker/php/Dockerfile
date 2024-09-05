FROM php:8.3-fpm

MAINTAINER info@lukashron.cz

WORKDIR /app

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# install required tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget gnupg g++ locales dialog apt-utils curl git zip unzip \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libonig-dev \
    libicu-dev \
    libxml2-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install necessary tools for running application
RUN docker-php-ext-install \
    bcmath \
    mbstring \
    fileinfo \
    intl \
    pdo \
    pdo_mysql \
    opcache \
    -j$(nproc) gd

RUN docker-php-ext-configure gd --with-freetype --with-jpeg

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install NPM
RUN apt-get update && apt-get install -y npm && apt-get clean

# Permission
RUN chown -R www-data:1000 /var/www && usermod -u 1000 www-data

USER www-data:1000