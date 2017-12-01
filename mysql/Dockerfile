FROM mysql/mysql-server:5.6

COPY ./my.cnf /etc/my.cnf
COPY ./cascade.sql /docker-entrypoint-initdb.d/db.sql

EXPOSE 3306
