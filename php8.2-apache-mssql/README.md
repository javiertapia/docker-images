# PHP8.2 Apache with SqlServer support

This image was made to be used as a base for another services or images.

- Base image: *php:8.2-apache**
- PHP modules: FreeTDS/DBLIB, PDO, Zip, Mbstring, Intl, Iconv, GD
- XDebug
- Composer

## Build the image

```bash
cd <this-repo-dir>
docker build -t javiertapia/php8.2-apache-mssql .
```