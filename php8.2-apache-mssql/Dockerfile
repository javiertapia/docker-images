FROM php:8.2-apache

# MAINTAINER javier.tapiad@global.komatsu

RUN apt-get update && apt-get install -y \
    unixodbc unixodbc-dev freetds-dev freetds-bin freetds-common tdsodbc curl gnupg \
    libonig-dev libmcrypt-dev \
    libpng-dev libfreetype6-dev libwebp-dev libjpeg62-turbo-dev \
    zlib1g-dev libicu-dev libzip-dev \
    g++ gcc make autoconf libc-dev pkg-config

RUN apt-get -y autoremove && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /usr/share/doc/*

# Config system

RUN rm /etc/localtime \
    && ln -s /usr/share/zoneinfo/America/Santiago /etc/localtime \
    && "date"

# The following RUN line run over Macos Silicon host architecture. 
# In Intel x64 hosts, replace `aarch64` with `x86_64`
RUN ln -s /usr/lib/aarch64-linux-gnu/libsybdb.so /usr/lib/ 

# PHP Extensions
RUN docker-php-ext-install -j$(nproc) iconv mysqli mbstring intl pdo pdo_mysql zip
RUN docker-php-ext-enable intl mbstring

RUN docker-php-ext-configure pdo_dblib --with-libdir=lib
RUN docker-php-ext-install -j$(nproc) pdo_dblib
RUN docker-php-ext-enable pdo_dblib

RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp
RUN docker-php-ext-enable intl mbstring
RUN cd /usr/src/php/ext/gd && make
# RUN cp /usr/src/php/ext/gd/modules/gd.so /usr/local/lib/php/extensions/no-debug-non-zts-20190902/gd.so
RUN docker-php-ext-install -j$(nproc) gd

# xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

# install Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer
ENV PATH /root/.composer/vendor/bin:$PATH

# config and run Apache
RUN a2enmod rewrite

EXPOSE 80 443
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]