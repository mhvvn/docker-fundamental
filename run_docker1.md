# Docker Run - Part 1 

1. Search image redis from dockerhub & harbor
* From DockerHub
docker search redis

* From Harbor
skopeo list-tags docker://registry.adinusa.id/btacademy/redis

2. Running image redis

```
docker run registry.adinusa.id/btacademy/redis  # CTRL + c for quit
docker run -d registry.adinusa.id/btacademy/redis  # Running in background (Background)
docker run -d --name redis1 registry.adinusa.id/btacademy/redis # Giving the container a name
```