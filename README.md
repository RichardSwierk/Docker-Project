# Docker-Project
Docker Project for SYS-265-05

### Demo video

[Demo](https://drive.google.com/file/d/1JAduY2qWyW4AyPe8qH2tOFwfWzm3PQ94/view?usp=sharing)

### Documentation

#### On docker01
```
root@ubuntu:# mkdir docker_wp
```
```
root@ubuntu:# cd docker_wp
```
```
root@ubuntu:# nano docker-compose.yml
```
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
```
root@ubuntu:# docker-compose up -d
```
```
root@ubunut:# docker ps -a
```
```
root@ubuntu:# nano docker-compose.yml
```
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
```
root@ubuntu:# docker-compose up -d
```
```
root@ubuntu:# docker ps -a
```
#### On mgmt01
http://docker01-name:8080/
Username: root
Password: root

#### On docker01
```
root@ubuntu:# nano docker-compose.yml
```
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
```
root@ubuntu:# docker-compose up -d
```
```
root@ubuntu:# docker ps -a
```

#### On mgmt01
http://docker01-name:80
Choose English
Title: Wordpress in Docker
Username: admin
Password: admin
Install WordPress
Click Plugins on the left side and remove “Hello Dolly” by:

#### On docker01
```
root@ubuntu:# cd wp-content/plugins/
```
```
root@ubuntu:# rm hello.php
```
*Removing default themes*
```
root@ubuntu:# cd wp-content/themes/
```
```
root@ubuntu:# rm -rf twenty*
```
*Getting our own theme*
```
root@ubuntu:# wget https://downloads.wordpress.org/theme/nisarg.1.2.6.zip
```
```
root@ubuntu:# unzip nisarg.1.2.6.zip
```
```
root@ubuntu:# rm -rf nisarg.1.2.6.zip
```
*if unzip isn’t installed*
```
root@ubuntu:# apt-get install -y unzip
```
#### On mgmt01
On the left side click Appearance and Themes and activate the Nisarg theme

#### On docker01
*We’re going to update WordPress*
```
root@ubuntu:# nano docker-compose.yml
```
Change image to WordPress:5.3.2
```
root@ubuntu:# docker stop dockerwp_wordpress_1
```
```
root@ubuntu:# docker rm dockerwp_wordpress_1
```
```
root@ubuntu:# docker-compose up -d
```
#### On mgmt01
Wait a minute for docker to reload
Click update now
Done

