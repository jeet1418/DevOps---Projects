FROM --platform=$TARGETPLATFORM mysql:8.0.33 AS build

ENV MYSQL_ROOT_PASSWORD="vprodbpass"
ENV MYSQL_DATABASE="accounts"

COPY db_backup.sql /docker-entrypoint-initdb.d/db_backup.sql