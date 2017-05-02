cascade on docker (with mysql)
===


Setup:

- download the manual installation files for cascade (cascade-x.x.zip)
  - place the unzipped folder inside the cas folder ("./cas/cascade-x.x")
- download the cascade sql file
  - place the sql file inside the mysql folder ("./mysql/cascade.sql")
- follow the manual installation instructions in cas/cascade-x.x/ManualConfiguration.txt
- edit docker-compose.yml, update the container name for the tomcat service with the hostname used in your license file

Initial Run:

- docker-compose build (this will build your containers)
- docker-compose up -d (spin up your containers and run them in the background)
- on your initial run, mysql may not be done importing the cascade.sql by the time tomcat starts to launch the cascade tomcat webapp
  - so you might want to run `docker-compose up mysql` first and let it finish doing it's initial import before bring cas up (`docker-compose up -d cas`)
- cascade should now be accesible via the docker host ip on port 8080
  - finish cascade setup by uploading your license file


personal upgrade notes:

- backup mysql database
  - from this directory:
  - docker exec cas_mysql_1 /usr/bin/mysqldump -u root --password=supersecret cascade > mysql/cascade.sql

  - old way:
  - docker exec -it <docker_id> /bin/bash
  - in mysql docker container:
    - mysqldump --databases cascade > cascade.sql -p
    - scp cascade.sql user@host:~/
- replace mysql/cascade.sql with new backup cascade.sql

- copy new cascade-x.x into tomcat/
- use install instructions to update necessary conf files:
    - tomcat/conf/context.xml
    - tomcat/conf/server.xml
    - tomcat/conf/web.xml
    - custom asset factory plugins and publish triggers

- update tomcat path for new cascade-x.x directory
- docker-compose build tomcat
- docker-compose up (add with -d for deamon mode)
