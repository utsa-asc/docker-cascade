cascade on docker (with mysql)
===


Setup:

- download the manual installation files for cascade 8.0.1
  - place the unzipped folder inside the cas folder ("./cas/cascade-8.0.1")
- download the cascade sql file
  - place the sql file inside the mysql folder ("./mysql/cascade.sql")
- follow the manual installation instructions in cas/cascade-8.0.1/ManualConfiguration.txt
- update the cas target in docker-compose.yml
  - make sure to set your hostname and extra_hosts (currently a hack) according to your license file
  - HOSTNAME should be the hostname used in your license file
  - extra_hosts should be defined as "<your_license_host_name>:<your_cas_docker_ip>"
      - you can get your cas docker container's ip by doing a docker inspect <container id>

Initial Run:

- docker-compose build (this will build your containers)
- docker-compose up -d (spin up your containers and run them in the background)
- on your initial run, mysql may not be done importing the cascade.sql by the time tomcat starts to launch the cascade tomcat webapp
  - so you might want to run `docker-compose up mysql` first and let it finish doing it's initial import before bring cas up (`docker-compose up -d cas`)
- cascade should now be running at localhost:8080, launch in a browser and upload your license file


personal upgrade notes:

- backup mysql database
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
