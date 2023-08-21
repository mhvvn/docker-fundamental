# Managing the Life Cycle of Containers - Part 1

## Docker Client Verbs
The Docker client, implemented by the docker command, provides a set of verbs to create and manage containers. The following figure shows a summary of the most commonly used verbs that change container state.

The Docker client also provides a set of verbs to obtain information about running and stopped containers. The following figure shows a summary of the most commonly used verbs that query information related to Docker containers.

Use these two figures as a reference while you learn about the docker command verbs along this course.

## Creating Containers

The docker run command creates a new container from an image and starts a process inside the new container. If the container image is not available, this command also tries to download it:

```
ubuntu@ubuntu20:~$ docker run -d nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
d121f8d1c412: Pull complete 
ebd81fc8c071: Pull complete 
655316c160af: Pull complete 
d15953c0e0f8: Pull complete 
2ee525c5c3cc: Pull complete 
Digest: sha256:c628b67d21744fce822d22fdcc0389f6bd763daac23a6b77147d0712ea7102d0
Status: Downloaded newer image for nginx:latest
bc17ead1a9a36294d511aa3ce40a667aa851b4337bbd404edb7b78087e81da54
```

Another important option is to run the container as a daemon, running the containerized process in the background. The -d option is responsible for running in detached mode.

The management docker commands require an ID or a name. The docker run command generates a random ID and a random name that are unique. The docker ps command is responsible for displaying these attributes:

CONTAINER ID This ID is generated automatically and must be unique.

NAMES This name can be generated automatically or manually specified.

If desired, the container name can be explicitly defined. The --name option is responsible for defining the container name:

```
ubuntu@ubuntu20:~$ docker run -d --name my-nginx-container nginx
```

Note The name must be unique. An error is thrown if another container has the same name, including containers that are stopped.

The container image itself specifies the command to run to start the containerized process, but a different one can be specified after the container image name in docker run:

```
ubuntu@ubuntu20:~$ docker run nginx ls /usr
bin
games
include
lib
local
sbin
share
src
```

The specified command must exist inside the container image.

Note Since a specified command was provided in the previous example, the nginx service does not start.

Sometimes it is desired to run a container executing a Bash shell. This can be achieved with:

```
ubuntu@ubuntu20:~$ docker run --name my-nginx-container -it nginx /bin/bash
root@190caba3547d:/# 
```

Options -t and -i are usually needed for interactive text-based programs, so they get a proper terminal, but not for background daemons.

### Running Commands in a Container

When a container is created, a default command is executed according to what is specified by the container image. However, it may be necessary to execute other commands to manage the running container. The docker exec command starts an additional process inside a running container:

```
ubuntu@ubuntu20:~$ docker exec 5d0c8fa3c1d9 cat /etc/hostname
5d0c8fa3c1d9
```
The previous example used the container ID to execute the command. It is also possible to use the container name:

```
ubuntu@ubuntu20:~$ docker exec my-nginx-container cat /etc/hostname
5d0c8fa3c1d9
```

### Demonstration: Creating Containers

Follow this step guide as the instructor shows how to create and manipulate containers.

1. Run the following command: 
```
ubuntu@ubuntu20:~$ docker run --name demo-container nginx dd if=/dev/zero of=/dev/null
```
This command downloads image the official Docker registry and starts it using the dd command. The container exits when the dd command returns the result. For educational purposes, the provided dd never stops.

2. Open a new terminal window from the node VM and check if the container is running:
```
ubuntu@ubuntu20:~$ docker ps
```

Some information about the container, including the container name demo-container specified in the last step, is displayed.

3. Open a new terminal window and stop the container using the provided name:
```
ubuntu@ubuntu20:~$ docker stop demo-container
```
4. Return to the original terminal window and verify that the container was stopped:
```
ubuntu@ubuntu20:~$ docker ps
```
5. Start a new container without specifying a name:
```
ubuntu@ubuntu20:~$ docker run nginx dd if=/dev/zero of=/dev/null
```
If a container name is not provided, docker generates a name for the container automatically.

6. Open a terminal window and verify the name that was generated
```
ubuntu@ubuntu20:~$ docker ps
An output similar to the following will be listed:
```
An output similar to the following will be listed:
```
ubuntu@ubuntu20:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
2ef4addadf8b        nginx               "/docker-entrypoint.…"   3 seconds ago       Up 1 second         80/tcp              busy_newton
```

The busy_newton is the generated name. you probably will have a different name for this step.

7. Stop the container with the generated name:

```
ubuntu@ubuntu20:~$ docker stop busy_newton
```

8. Containers can have a default long-running command. For these cases, it is possible to run a container as a daemon using the -d option. For example, when a MySQL container is started it creates the databases and keeps the server actively listening on its port. Another example using dd as the long-running command is as follows:

```
ubuntu@ubuntu20:~$ docker run --name demo-container-2 -d nginx dd if=/dev/zero of=/dev/null
```

9. Stop the container
```
ubuntu@ubuntu20:~$ docker stop demo-container-2
```

10. Another possibility is to run a container to just execute a specific dommand:
```
ubuntu@ubuntu20:~$ docker run --name demo-container-3 nginx ls /etc
```
This command starts a new container, lists all files available in the /etc directory in the container, and exits.

11. Verify that the container is not running:
```
ubuntu@ubuntu20:~$ docker ps
```

12. It is possible to run a container in interactive mode. This mode allows for staying in the container when the container runs:

```
ubuntu@ubuntu20:~$ docker run --name demo-container-4 -it nginx /bin/bash
```
the -i option specifies that this container should run in interactive mode, and the -t allocates a pseudo-TTY.

13. Exit the Bash shell from the cintainer:

```
root@7e202f7faf07:/# exit
exit
```

14. Remove all stopped containers from the environment by running the following from a terminal window:

```
ubuntu@ubuntu20:~$ docker ps -a
```

```
ubuntu@ubuntu20:~$ docker rm demo-container demo-container-2 demo-container-3 demo-container-4
```

15. Remove the container started without a name. Replace the with the container name from the step 7:

```
ubuntu@ubuntu20:~$ docker rm <container_name>

```
16. Remove the nginx container image:

```
ubuntu@ubuntu20:~$ docker image ls
ubuntu@ubuntu20:~$  docker image rm nginx
```