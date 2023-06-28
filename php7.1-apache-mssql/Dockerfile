FROM php:7.1-apache

# MAINTAINER Javier Tapia <javier.tapiad@global.komatsu>

### Update OS and install libraries
RUN apt-get update && apt-get install -y \
    git zlib1g-dev unixodbc unixodbc-dev freetds-dev freetds-bin \
    tdsodbc curl gnupg libicu-dev libpng-dev zlib1g-dev unzip;

RUN apt-get -y autoremove && apt-get clean && \
    rm -rf /var/lib/apt/lists/*;

# Config system
RUN rm /etc/localtime \
    && ln -s /usr/share/zoneinfo/America/Santiago /etc/localtime \
    && "date"

RUN ln -s /usr/lib/aarch64-linux-gnu/libsybdb.so /usr/lib/

RUN set -x \
    && cd /usr/src/ && tar -xf php.tar.xz && mv php-7* php \
    && cd /usr/src/php/ext/odbc \
    && phpize \
    && sed -ri 's@^ *test +"\$PHP_.*" *= *"no" *&& *PHP_.*=yes *$@#&@g' configure \
    && ./configure --with-unixODBC=shared,/usr > /dev/null \
    && docker-php-ext-install odbc > /dev/null

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_dblib intl mbstring zip gd
RUN docker-php-ext-configure pdo_dblib --with-libdir=lib
RUN docker-php-ext-enable intl mbstring pdo_dblib

# stdout Apache access & error log
RUN ln -sf /proc/self/fd/1 /var/log/apache2/access.log \
    && ln -sf /proc/self/fd/1 /var/log/apache2/error.log

# install xdebug
RUN yes | pecl install xdebug-2.6.0 \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini

### Install Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

RUN a2enmod rewrite
RUN a2enmod ssl

EXPOSE 80 443
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]