# PHP7.4 CLI with SqlServer support

This image was made to be used as a base for another services or images.

* Base image: **php:7.1-apache**
* PHP modules: FreeTDS/DBLIB, PDO, Zip, Intl, Iconv, GD
* Composer
* PHPUnit

## Build the image

```bash
cd <this-repo-dir>
docker build -t javiertapia/php7.4-cli-mssql .
```

## Usage aside many services

The **docker-compose.yml** file provides an example of using this Dockerfile aside an SqlServer/AzureSql service.
