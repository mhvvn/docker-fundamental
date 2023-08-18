# Runnung Docker Part III
## Practice 05

1. Running mysql container with additional parameters.

```
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=RAHASIA -e MYSQL_DATABASE=latihan05 -p 3306:3306 \
registry.adinusa.id/btacademy/mysql
```
2. Pull image phpmyadmin from harbor.
```
docker pull registry.adinusa.id/btacademy/phpmyadmin:latest
```
3. Running phpmyadmin container and connect it with mysql container.
```
docker run --name my-phpmyadmin -d --link my-mysql:db -p 8090:80 \
registry.adinusa.id/btacademy/phpmyadmin
```

4. Test browsing.
```
open in browser http://10.10.10.11:8090 login with user: `root` dan password: `RAHASIA` 
```
## practice 06
1. Run ubuntu containers with names ubuntu1 & ubuntu2.
```
docker run -dit --name ubuntu1 registry.adinusa.id/btacademy/ubuntu
docker run -dit --name ubuntu2 registry.adinusa.id/btacademy/ubuntu
```
2. Display list container.
```
docker ps
```
3. Pause container ubuntu.
```
docker pause ubuntu1
docker pause ubuntu2
```
4. Check in list container if the container status is paused.
```
docker ps
```
5. Check resource usage when the ubuntu container is paused.
```
docker stats ubuntu1
docker stats ubuntu2
```
6. Unpause container ubuntu1.
```
docker unpause ubuntu1
```
## practice 07
1. Create database containers with limited specifications.
```
docker container run -d --name ch6_mariadb --memory 256m --cpu-shares 1024 --cap-drop net_raw \
 -e MYSQL_ROOT_PASSWORD=test registry.adinusa.id/btacademy/mariadb:5.5
```
2. Create a wordpress container and connect it to the database container.
```
docker container run -d -p 80:80 -P --name ch6_wordpress  --memory 512m --cpu-shares 512 \
--cap-drop net_raw --link ch6_mariadb:mysql -e WORDPRESS_DB_PASSWORD=test \
registry.adinusa.id/btacademy/wordpress:5.0.0-php7.2-apache
```
3. Check logs, running process, and resource.
```
docker logs ch6_mariadb
docker logs ch6_wordpress

docker top ch6_mariadb
docker top ch6_wordpress

docker stats ch6_mariadb
docker stats ch6_wordpress
```
4. Test browsing.
```
Open in browser http://10.10.10.11 then complete the installation
```
### Verification
- Can access wordpress using English
- phpmyadmin can be accessed
- Status ubuntu2 paused
- wordpress container specs as required