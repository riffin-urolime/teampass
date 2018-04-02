# teampass

Hello,

Here are the steps to dockerize the teampass:

Requirements:

1. A linux server
2. docker and docker-compose installed


Steps:

1. Change to teampass document root
2. mkdir volumes volumes/teampass volumes/mysql
3. nano docker-compose.yml
=================
version: "2"
services:
  teampass:
      image: teampass/teampass
      ports:
        - "80:80"
        - "443:443"
      volumes:
        - "./volumes/teampass:/var/www/html"
      environment:
        VIRTUAL_HOST: teampass.domain.com
      links:
        - db
      
  db:
      image: mysql:latest
      ports:
          - "3306:3306"
      volumes:
          - "./volumes/mysql:/var/lib/mysql"
      environment:
          - MYSQL_DATABASE=teampass
          - MYSQL_PASSWORD=qwerty
          - MYSQL_ROOT_PASSWORD=qwe123!@#
          - MYSQL_USER=teampass

=================

4. docker-compose up &
5. Access the virtual host in browser and follow up to complete the teampass installation.

Troubleshooting:

If you are unable to access teampass after installation, or you receive the following error:
"""""""" Table 'teampass.teampass_restriction_to_roles' doesn't exist  """"""""""

1. access the mysql docker container
2. Login to mysql command line and select the teampass database 
3. Execute the following command line:
***
CREATE TABLE teampass_restriction_to_roles ( `id` INT NOT NULL AUTO_INCREMENT , `role_id` INT NOT NULL , `item_id` INT NOT NULL , PRIMARY KEY (`id`)) ENGINE = InnoDB;
***
