FROM tomcat:9-jdk11

COPY ./context.xml /usr/local/tomcat/conf/context.xml
COPY ./server.xml /usr/local/tomcat/conf/server.xml
##COPY ./webapps/ROOT /usr/local/tomcat/webapps/ROOT
RUN mkdir -p "/usr/local/tomcat/indexes"

EXPOSE 8080
EXPOSE 8443
