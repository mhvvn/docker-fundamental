# Running Docker Part-I

## Practice 01

1. Search image redis from dockerhub & harbor
* #From DockerHub
```
docker search redis
```

* #From Harbor
```
skopeo list-tags docker://registry.adinusa.id/btacademy/redis
```
2. Running image redis

```
docker run registry.adinusa.id/btacademy/redis  # CTRL + c for quit
docker run -d registry.adinusa.id/btacademy/redis  # Running in background (Background)
docker run -d --name redis1 registry.adinusa.id/btacademy/redis # Giving the container a name
```
3. Display running container.
```
docker ps 
docker container ls
```
4. Display all docker container.
```
docker ps -a
docker container ls -a
```
5. Display container description .
```
# docker inspect CONTAINER_NAME CONTAINER_ID
docker inspect redis1
```
6. Display content of the logs in container.
```
# docker logs CONTAINER_NAME/CONTAINER_ID
docker logs redis1
```

7. Display live stream resource used in container.
```
# docker stats CONTAINER_NAME/CONTAINER_ID
docker stats redis1
```
8. Display running process in container.
```
# docker top CONTAINER_NAME/CONTAINER_ID
docker top redis1
```
9. Shutdown the container.
```
# docker stop CONTAINER_NAME/CONTAINER_ID
docker stop redis1
```
## Practice 02
1. Search image nginx From DockerHub & Harbor

```
# From DockerHub
docker search nginx

# From Harbor
skopeo list-tags docker://registry.adinusa.id/btacademy/nginx
```

2. Running image nginx and expose to port host.
```
docker run -d --name nginx1 -p 80:80 registry.adinusa.id/btacademy/nginx:latest
```

3. Display a description of the nginx container.
```
docker inspect nginx1
```
4. Running image nginx and declare the container port.
```
docker run -d --name nginx2 -p 80 registry.adinusa.id/btacademy/nginx:latest
```

5. Test browsing.
```
curl localhost:$(docker port nginx2 80 | cut -d : -f 2)
```
6. Display container (activate/exit).

```
docker ps -a 
docker container ls -a
```
7. Display image docker.
```
docker images
```
### Verification
- Can access with curl to nginx1 & nginx2 containers
- Container redis1 running