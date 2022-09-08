# PHP7.4 Apache with SqlServer support

This image was made to be used as a base for another services or images.

- Base image: *php:7.4-apache**
- PHP modules: FreeTDS/DBLIB, PDO, Zip, Mbstring, Intl, Mysqli, Iconv, GD
- XDebug
- Composer

## Build the image

```bash
cd <this-repo-dir>
docker build -t javiertapia/php7.4-apache-mssql .
```

## Usage aside many services

In the following docker-compose.yml example, this Dockerfile is used as a base of many services. This allow re-utilize the code, withoout increase disk space due duplication.

```yaml
version: '3.2'

services:
  # ...
  auth:
    image: javiertapia/php7.4-apache-mssql # this image
    ports:
      - "8059:80"
    expose:
      - 80
    volumes:
      - ./:/www
      - ./files/auth.conf:/etc/apache2/sites-available/000-default.conf
      - ./files/php-custom.ini:/usr/local/etc/php/conf.d/custom.ini
  frontend:
    image: javiertapia/php7.4-apache-mssql # this image
    ports:
      - "8060:80"
    expose:
      - 80
    volumes:
      - ./:/www
      - ./files/frontend.conf:/etc/apache2/sites-available/000-default.conf
      - ./files/php-custom.ini:/usr/local/etc/php/php.ini
      - ./files/php-xdebug.ini:/usr/local/etc/php/conf.d/xdebug-local.ini
    environment:
      TZ: America/Santiago
      PHP_MEMORY_LIMIT: 128M
      XDEBUG_SESSION: PHPSTORM
  # Other services...
```

The auth and frontend services uses the same base image, but can use different config files or environment variables.
