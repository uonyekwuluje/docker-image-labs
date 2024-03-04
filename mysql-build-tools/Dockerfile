FROM mysql/mysql-server:5.7
MAINTAINER Uchechukwu Onyekwuluje <uionyekwuluje@gmail.com>

ENV MYSQL_DATABASE studentdb

COPY ./mysql-scripts/ /docker-entrypoint-initdb.d/
