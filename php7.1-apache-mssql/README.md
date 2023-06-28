# PHP7.1 Apache with SqlServer support

This image was made to be used as a base for another services or images.

* Base image: **php:7.1-apache**
* PHP modules: FreeTDS/DBLIB, PDO, Zip, Mbstring, Intl, Mysqli, Iconv, GD
* XDebug
* Composer

## Build the image

```bash
cd <this-repo-dir>
docker build -t javiertapia/php7.4-apache-mssql .
```

## Usage aside many services

The **docker-compose.yml** file provides an example of using this Dockerfile aside an SqlServer/AzureSql service.
