# javiertapia docker-files

javiertapia repository to store `Dockerfile`s, `docker-compose.yml`s and other files to develop proyects over Docker containers.

## How to used

Follow README.md files located each Docker image directory.

The images must be builded using the commands:

```bash
cd <docker image dir>
docker build -t <dir-name> .
```

## TODO

- [x] PHP8.3 + Apache + MsSQL(DBLIB) support
- [x] PHP8.2 + Apache + MsSQL(DBLIB) support
- [x] PHP8.1-FPM + Mysql support
- [x] PHP7.4 + Apache + MsSQL(DBLIB) support
- [x] PHP7.4 CLI + MsSQL(DBLIB) support
- [x] PHP7.1 + Apache + MsSQL(DBLIB) support
- [ ] Node + Angular CLI development environment
- [ ] Nginx Angular production
- [ ] Node + NestJs development
- [ ] Node + NestJs production
