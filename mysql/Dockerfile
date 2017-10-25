FROM mysql/mysql-server:5.6

ENV MYSQL_ROOT_PASSWORD supersecret
ENV MYSQL_DATABASE cas_test
ENV MYSQL_USER cas
ENV MYSQL_PASSWORD supersecret
ENV MYSQL_ROOT_HOST : %

EXPOSE 3306

COPY ./my.cnf /etc/my.cnf
COPY ./cascade.sql /docker-entrypoint-initdb.d/db.sql
