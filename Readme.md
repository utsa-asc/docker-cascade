cascade on docker (with mysql)
===

__Setup:__

- download the manual installation files for cascade (cascade-x.x.zip or ROOT.zip) [https://www.hannonhill.com/downloads/cascade/]
  - place the unzipped tomcat/webapps/ROOT folder inside the tomcat webapps directory ("./tomcat/webapps/ROOT")
- download the cascade sql file
  - place the sql file inside the mysql folder ("./mysql/cascade.sql")
- follow the manual installation instructions in cas/cascade-x.x/ManualConfiguration.txt
  - edit context.xml and server.xml to your liking, the mysql database will be reachable at the host: ```"mysql"```
- edit docker-compose.yml, update the container name for the tomcat service with the hostname used in your license file
  - ```containter_name: <license host name>```

__Initial Run:__

- ```docker-compose build``` (this will build your containers)
- ```docker-compose up -d```(this will spin up your containers and run them in the background)
- cascade should now be accesible via the docker host ip on port 8080
  - you can use ```docker-compose up``` if you run into problems, this will attach the docker containers to your stdout and you should be able to see all the log output
  - finish cascade setup by uploading your license file

__Notes:__

- no longer needed (tomcat service definition should have a depends_on mysql), but keeping this here just in case:
  - on your initial run, mysql may not be done importing the cascade.sql by the time tomcat starts to launch the cascade tomcat webapp
  - so you might want to run ```docker-compose up mysql``` first and let it finish doing it's initial import before bringing up cascade (`docker-compose up -d cas`)


__Personal upgrade/Other notes:__
- container_name in docker-compose.yml is used to sync the cascade host name with
  your cascade license file obtained from HannonHill, make sure the hostname in the license
  file matches your container_name
    - this results in a side effect of limiting you to one cascade container with
      container_name, i.e.-- no clustering =(

- backup mysql database
  - from this directory:
  - ```docker exec <mysql container id> /usr/bin/mysqldump -u root --password=supersecret cascade > mysql/cascade.sql```
- if you need ever need to rebuild the mysql container, use a previous mysql dump backup
  by placing the mysqldump output in ./mysql/cascade.sql


__Upgrade cascade flow:__

- copy new ROOT app into ./tomcat/webapps/ROOT
- use install instructions to update necessary conf files:
    - tomcat/conf/context.xml
    - tomcat/conf/server.xml
    - tomcat/conf/web.xml
    - custom asset factory plugins and publish triggers
- rebuild the new cascade version that tomcat uses:
  - docker-compose build tomcat
  - docker-compose up (add with -d for deamon mode)

- now with ssl!
  - the included server.xml has a working SSL config with local generated certificate
    - Place the cert and key in the ./tomcat/ssl folder, update server.xml config to reflect
      ```
      <Certificate certificateKeyFile="ssl/localhost-docker-key.pem"
                   certificateFile="ssl/localhost-docker-cer.pem"
                   certificateKeyPassword="changeit"
                   type="RSA" />
      ```
    - the docker-compose.yml is configured to reach the HTTPS listener on :8443
