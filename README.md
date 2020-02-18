# Docker-Project
Docker Project for SYS-265-05

### Demo video

[Demo](https://drive.google.com/file/d/1JAduY2qWyW4AyPe8qH2tOFwfWzm3PQ94/view?usp=sharing)

### Documentation

#### On docker01
Create the WordPress directory
```
root@ubuntu:# mkdir docker_wp
```
Navigate to the newly created directory
```
root@ubuntu:# cd docker_wp
```
Create docker-compose.yml
```
root@ubuntu:# nano docker-compose.yml
```
Add the following to the docker-compose.yml
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
  database_data:
```
Save the docker-compose.yml file and exit it <br />
<br />Build the containers and have them run in the background
```
root@ubuntu:# docker-compose up -d
```
Display all running and stopped containers
```
root@ubunut:# docker ps -a
```
Edit the docker-compose.yml again
```
root@ubuntu:# nano docker-compose.yml
```
Add the follwing after database configuration and before empty networks line:
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
Recreate the containers and run them in the background again
```
root@ubuntu:# docker-compose up -d
```
Display all running and stopped containers
```
root@ubuntu:# docker ps -a
```
#### On mgmt01
Ensure that you can successfully navigate to the server and log in (phpMyAdmin):<br />
http://docker01-name:8080/<br />
Username: root<br />
Password: root

#### On docker01
Edit docker-compose.yml once again
```
root@ubuntu:# nano docker-compose.yml
```
Add the follow WordPress configuration immediately after the initial services line and right before the database configuration:
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
Recreate the containers and run them in the background
```
root@ubuntu:# docker-compose up -d
```
Display all running and stopped containers
```
root@ubuntu:# docker ps -a
```

#### On mgmt01
http://docker01-name:80/<br />
Choose English<br />
Title: Wordpress in Docker<br />
Username: admin<br />
Password: admin<br />
Confirm Password<br />
Enter your email address<br />
Install WordPress<br />
Login using previously created credentials<br />
Click Plugins on the left side, click Installed Plugins, and delete “Hello Dolly”

#### On docker01
Ensure "Hello Dolly has been removed
```
root@ubuntu:# cd wp-content/plugins/
```
```
root@ubuntu:# ls
```
If "hello.php" is present in the directory, go ahead and remove it:
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
Navigate back to phpMyAdmin (http://docker01-name:8080/)<br />
Log in using the previously created credentials<br />
Click the arrow in the top left to show the side panel<br />
Expand wordpress<br />
Select wp_options<br />
Navigate to the second page of options and find the template and stylesheet options<br />
Change both from 'twentysixteen' to 'nisarg'<br />
Return to http://docker01-name:80/ to ensure the new theme has been applied<br />

#### On docker01
*We’re going to update WordPress*
Edit docker-compose once again
```
root@ubuntu:# nano docker-compose.yml
```
Change image from wordpress:4.6 to wordpress:5.3.2<br />
Remove the previous dockerwp_wordpress_1 and create the updated containers
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

