FROM java:openjdk-8-jre-alpine

ENV CAS_HOME /opt/cas
ENV CATALINA_HOME /opt/cas/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH

RUN mkdir -p "$CAS_HOME"
COPY ./cascade-8.6.1 /opt/cas
RUN mkdir -p "$CAS_HOME/tomcat/indexes"

WORKDIR $CATALINA_HOME
EXPOSE 8080
CMD ["catalina.sh", "run"]
