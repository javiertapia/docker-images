FROM php:7.4-cli

### Update OS and install libraries
RUN apt-get update && apt-get install -y \
    zlib1g-dev unixodbc unixodbc-dev freetds-dev freetds-bin \
    libpng-dev libfreetype6-dev libwebp-dev libjpeg62-turbo-dev \
    tdsodbc curl gnupg libicu-dev libpng-dev zlib1g-dev unzip libzip-dev;

RUN apt-get -y autoremove && apt-get clean && \
    rm -rf /var/lib/apt/lists/*;

# Config system
RUN rm /etc/localtime \
    && ln -s /usr/share/zoneinfo/America/Santiago /etc/localtime \
    && "date"

# RUN ln -s /usr/lib/x86_64-linux-gnu/libsybdb.so /usr/lib/
RUN cp /usr/lib/aarch64-linux-gnu/libsybdb.so /usr/lib/

RUN set -x \
    && cd /usr/src/ && tar -xf php.tar.xz && mv php-7* php \
    && cd /usr/src/php/ext/odbc \
    && phpize \
    && sed -ri 's@^ *test +"\$PHP_.*" *= *"no" *&& *PHP_.*=yes *$@#&@g' configure \
    && ./configure --with-unixODBC=shared,/usr > /dev/null \
    && docker-php-ext-install odbc > /dev/null

# PHP Extensions
RUN docker-php-ext-install -j$(nproc) iconv intl pdo pdo_mysql zip
RUN docker-php-ext-enable intl

RUN docker-php-ext-configure pdo_dblib --with-libdir=lib
RUN docker-php-ext-install -j$(nproc) pdo_dblib
RUN docker-php-ext-enable pdo_dblib

RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp
RUN docker-php-ext-enable intl
RUN cd /usr/src/php/ext/gd && make
RUN cp /usr/src/php/ext/gd/modules/gd.so /usr/local/lib/php/extensions/no-debug-non-zts-20190902/gd.so
RUN docker-php-ext-install -j$(nproc) gd


### Install Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

### Install phpunit
RUN composer global require "phpunit/phpunit:^7.3"
ENV PATH /root/.composer/vendor/bin:$PATH
RUN ln -s /root/.composer/vendor/bin/phpunit /usr/bin/phpunit