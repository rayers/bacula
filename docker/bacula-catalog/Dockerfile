FROM --platform=linux/amd64 dividia/bacula-base:11.0.5 AS base

FROM mariadb:10.3

MAINTAINER eduardo@fametec.com.br

ENV MYSQL_HOST db

ENV MYSQL_PASSWORD bacula

ENV MYSQL_USER bacula

ENV MYSQL_DATABASE bacula

ENV MYSQL_INITDB_ARGS '--encoding=SQL_ASCII --lc-collate=C --lc-ctype=C'

COPY --from=base /opt/bacula/scripts/make_mysql_tables /docker-entrypoint-initdb.d/make_mysql_tables

COPY --from=base /opt/bacula/scripts/grant_mysql_privileges /docker-entrypoint-initdb.d/grant_mysql_privileges

RUN { \
	echo '#!/bin/bash'; \
	echo 'sh /docker-entrypoint-initdb.d/make_mysql_tables --host=$MYSQL_HOST --user=$MYSQL_USER --password=$MYSQL_PASSWORD'; \
	echo 'sh /docker-entrypoint-initdb.d/grant_mysql_privileges --host=$MYSQL_HOST --user=$MYSQL_USER --password=$MYSQL_PASSWORD'; \
} >> /docker-entrypoint-initdb.d/deploy_database.sh \
&& chmod +x /docker-entrypoint-initdb.d/deploy_database.sh \
&& chown mysql. /docker-entrypoint-initdb.d/deploy_database.sh

# COPY 01-make_mysql_tables.sql 02-grant_mysql_privileges.sql /docker-entrypoint-initdb.d/

RUN chown -R mysql. /docker-entrypoint-initdb.d/*

VOLUME ["/var/lib/mysql/data"]

EXPOSE 5432/tcp

