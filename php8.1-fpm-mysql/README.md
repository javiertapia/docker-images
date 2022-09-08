# PHP8.1 FPM Alpine with MySQL support

This image was made to be used as a base for another services or images.

* Base image: **php:8.1-fpm-alpine**
* PHP modules: mysqli, PDO (mysql), Zip
* XDebug
* Composer

## Build the image

```bash
cd <this-repo-dir>
docker build -t javiertapia/php8.1-fpm-mysql .
```

## Usage with a reverse proxy

In the following `docker-compose.yml` example, this Dockerfile is used to build a "php" service. Another service uses Apache http server to function as a "reverse proxy".

```yaml
version: '3.7'

services:
  php:
    image: javiertapia/php8.1-fpm-mysql # this image, builded with `docker build`
    environment:
      PHP_MEMORY_LIMIT: '128M'
   # ...
  frontend:
    image: httpd:2.4.46-alpine # Apache HTTP server
    ports:
      - "8888:80"
    volumes:
      - ./httpd-local.conf:/usr/local/apache2/conf/httpd.conf # here is defined the reverse proxy
    links:
      - php # link to this image
    # ...
  # ...
```

In the previous example, the file `httpd-local.conf` define the reverse proxy inside the Apache Server configuration:

```bash
# ...
# The following lines must be written after the default httpd.conf directives.

<VirtualHost *:80>
    # Proxy .php requests to port 9000 of the php-fpm container
    ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://php:9000/www/html/public/$1
    DocumentRoot /www/html/public/
    <Directory /www/html/public/>
        DirectoryIndex index.php
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    # ... other configurations
</VirtualHost>
```