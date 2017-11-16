FROM tomcat:8-alpine

COPY ./context.xml /usr/local/tomcat/conf/context.xml
COPY ./webapps/ROOT /usr/local/tomcat/webapps/ROOT
RUN mkdir -p "/usr/local/tomcat/indexes"

EXPOSE 8080
