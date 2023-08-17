# Docker Run - Part 2

## practice 03
1. Search image nginx from DockerHub & 
```
# From DockerHub
docker search nginx

# From Harbor
skopeo list-tags docker://registry.adinusa.id/btacademy/nginx
```

2. Running an nginx image with the name nginx1 and expose port 8080.
```
docker run -d --name nginx1 -p 8080:80 registry.adinusa.id/btacademy/nginx:latest
```
3. Displays a description of the nginx1 container.
```
docker inspect nginx1
```
4. Running an nginx image with the name nginx2 and expose port 8081.
```
docker run -d --name nginx2 -p 8081:80 registry.adinusa.id/btacademy/nginx:latest
```

5. Display container (activate/exit).
```
docker ps -a 
docker container ls -a
```

6. Check nginx output on containers.
```
curl localhost:8080
curl localhost:8081
```
7. Accessing the container.
```
docker exec -it nginx2 /bin/bash
```
8. Update and install editor on the container.
```
apt-get update -y && apt-get install nano -y
```
9. Edit index.html and move index.html to default directory nginx
```
nano index.html
...
hello, username.
...

mv index.html /usr/share/nginx/html
```

10. Restart service nginx.
```
service nginx restart
```

11. Rerun the container.
```
docker start nginx2
```
12. Display container.
```
docker ps
docker container ls
```
13. Check nginx output on containers.

```
curl localhost:8080
curl localhost:8081
```

14. Display a description of the container .
```
docker inspect nginx1
docker inspect nginx2
```
15. Display the log content in the container.
```
docker logs nginx1
docker logs nginx2
```

16. Displays the live resources used in the container.
```
docker stats nginx1
docker stats nginx2
```
17. Display running process in container.
```
docker top nginx1
docker top nginx2
```

## practice 04
1. Search image ubuntu from DockerHub & Harbor
```
# From DockerHub
docker search ubuntu

# From Harbor
skopeo list-tags docker://registry.adinusa.id/btacademy/ubuntu
```
2. Pull image ubuntu from harbor
```
docker pull registry.adinusa.id/btacademy/ubuntu
```

3. Running the ubuntu container and access to the console.

```
docker run -it --name ubuntu1 registry.adinusa.id/btacademy/ubuntu

Exit the container with Ctrl+D or exit

docker ps -a
```
4. Run the ubuntu container and delete it when exiting the container.

```
docker run -it --rm --name ubuntu2 registry.adinusa.id/btacademy/ubuntu

Exit the container with Ctrl+D or exit

docker ps -a
```

### Verification
- Can curl into nginx1 [default nginx]
- Can curl into nginx2 and output nginx2 [hello, username.]
- Ubuntu1 container Exited