# Docker-Project
Docker Project for SYS-265-05

### Demo video

[Demo](https://drive.google.com/file/d/1JAduY2qWyW4AyPe8qH2tOFwfWzm3PQ94/view?usp=sharing)

### Documentation

#### On docker01
Mkdir docker_wp
Cd docker_wp

Nano docker-compose.yml
```
version: '2'
services:
database:
image: mysql:5.7.17
restart: always
volumes:
- database_data:/var/lib/mysql
environment:
MYSQL_ROOT_PASSWORD: root
networks:
- back
networks:
back:
volumes:
Database_data:
```

docker-compose up -d
Docker ps -a

Nano docker-compose.yml
```
phpmyadmin:
depends_on:
- database
image: phpmyadmin/phpmyadmin
restart: always
ports:
- 8080:80
environment:
PMA_HOST: database
MYSQL_ROOT_PASSWORD: root
networks:
- back
```

Docker-compose up -d
Docker ps -a

#### On mgmt01
http://docker01-name:8080/
Username: root
Password: root

#### On docker01
Nano docker-compose.yml
```
wordpress:
depends_on:
- database
image: wordpress:4.6
restart: always
volumes:
- ./wp-content:/var/www/html/wp-content
environment:
WORDPRESS_DB_HOST: database:3306
WORDPRESS_DB_PASSWORD: root
ports:
- 80:80
- 443:443
networks:
- back
```

Docker-compose up -d
Docker ps -a

#### On mgmt01
http://docker01-name:80
Choose English
Title: Wordpress in Docker
Username: admin
Password: admin
Install WordPress
Click Plugins on the left side and remove “Hello Dolly” by:

#### On docker01
Cd wp-content/plugins/
Rm hello.php

*Removing default themes*
Cd wp-content/themes/
Rm -rf twenty*

*Getting our own theme*
Wget https://downloads.wordpress.org/theme/nisarg.1.2.6.zip
Unzip nisarg.1.2.6.zip
Rm -rf nisarg.1.2.6.zip

*if unzip isn’t installed*
Sudo apt-get install -y unzip

#### On mgmt01
On the left side click Appearance and Themes and activate the Nisarg theme

#### On docker01
*We’re going to update WordPress*

Nano docker-compose.yml
Change image to WordPress:5.3.2

Docker stop dockerwp_wordpress_1
Docker rm dockerwp_wordpress_1
Docker-compose up -d

#### On mgmt01
Wait a minute for docker to reload
Click update now
Done

