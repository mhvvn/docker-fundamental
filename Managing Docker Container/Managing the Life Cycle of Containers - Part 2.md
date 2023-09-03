# Managing Containers

Docker provides the following commands to manage containers :
docker ps : This command is responsible for listing running containers.

CONTAINER ID Each container, when created, gets a container ID, which is a hexadecimal number and looks like an image ID, but is actually unrelated.

IMAGE Container image that was used to start the container.

COMMAND Command that was executed when the container started.

CREATED Date and time the container was started.

STATUS Total container uptime, if still running, or time since terminated.

PORT Ports that were exposed by the container or the port forwards, if configured.

NAMES The container name.

Stopped containers are not discarded immediately. Their local file systems and other states are preserved so they can be inspected for post-mortem analysis. Option -a lists all containers, including containers that were not discarded yet:

```
ubuntu@ubuntu20:~$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
2ef4addadf8b        nginx               "/docker-entrypoint.…"   18 hours ago        Exited (137) 18 hours ago                       busy_newton
9aa8f1ff1c03        nginx               "/docker-entrypoint.…"   21 hours ago        Up 21 hours                 80/tcp              my-nginx-container
```

docker inspect: This command is responsible for listing metadata about a running or stopped container. The command produces a JSON output

```
ubuntu@ubuntu20:~$ docker inspect my-nginx-container
[
    {
        "Id": "9aa8f1ff1c0399cbb24b657de6ce0a078c7ce69dc0c07760664670ea7593aaf3",
...OUTPUT OMITTED....
"NetworkSettings": {
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "9e91a2e58d8a7c21532b0a48bb4d36093a347aa056e85d5b7f3941ff19eeb840",
                    "EndpointID": "9d847b45ad4c55b2c1fbdaa8f44a502f2215f569c08cbcc6599b6ff1c25a2869",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
...OUTPUT OMITTED....
```

This command allows formatting of the output string using the given go template with the -f option. For example, to retrieve only the IP address, the following command can be executed: 
```
ubuntu@ubuntu20:~$ docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress }}' $(docker ps -aq)
/busy_newton - 
/my-nginx-container - 172.17.0.2
```

docker stop: This command is responsible for stopping a running container gracefully:
```
$ docker stop my-nginx-container
```

Using docker stop is easier than finding the container start process on the host OS and killing it.

docker kill: This command is responsible for stopping a running container forcefully:

```
$ docker kill my-nginx-container
```
It is possible to specify the signal with the -s option:

```
$ docker kill -s SIGKILL my-nginx-container
```

The following signals are available

| SIGNAL | DEFAULT ACTION | DESCRIPTION |
| ----------- | :---------: | ----------: |
| SIGHUP | Terminate process | Terminate line hangup |
| SIGINT | Teriminate process | Interrupt program |
| SIGQOUT | Create core image | Quit program |
| SIGABRT | Create core image | Abort program |
| SIGKILL | Terimate process | Abort program |
| SIGTERM | Terminate process | Software termination signal |
| SIGUSR1 | Terminate process | User-defined signal 1 |
| SIGUSR2 | Terminate process | User-defined signal 2 |

docker restart: This command is responsible for restarting a stopped container:

```
$ docker restart my-nginx-container
```
The docker restart command creates a new container with the same container ID, reusing the stopped container state and filesystem.

docker rm: This command is responsible for deleting a container, discarding its state and filesystem:

```
$ docker rm my-nginx-container
```

It is possible to delete all containers at the same time. The docker ps command has the -q option that returns only the ID of the containers. This list can be passed to the docker rm command:

```
$ docker rm $(docker ps -aq)
```

Before deleting all containers, all running containers must be stopped. It is possible to stop all containers with:

```
$ docker stop $(docker ps -q)
```

Note The commands docker inspect, docker stop, docker kill, docker restart, and docker rm can use the container ID instead of the container name.

Demonstration: Managing a Container

Following this guide as the instructor shows how to manage a container.

1.Run the following command:
```
$ docker run --name demo-container -d nginx
```

This command will start a nginx container as a daemon.

2.List all running containers:
```
$ docker ps
```
3.Stop the container with the following command:
```
$ docker stop demo-container
```

4.Verify that the container is not running:

```
$ docker ps
```

5.Run a new container with the same name:

```
$ docker run --name demo-container -d  nginx
```

A conflict error is displayed. Remember that a stopped container is not discarded immediately and their local file systems and other states are preserved so they can be inspected for post-mortem analysis

6.It is possible to list all containers with the following command:

```
$ docker ps -a
```

7.Start a new nginx container:

```
$ docker run --name demo-1-nginx -d nginx
```

8.An important feature is the ability to list metadata about a running or stopped container. The following command returns the metadata:

```
$ docker inspect demo-1-nginx
```

9.It is possible to format and retrieve a specific item from the inspect command. To retrieve the IPAddress attribute from the NetworkSettings object, use the following command:

```
$ docker inspect -f '{{ .NetworkSettings.IPAddress }}' demo-1-nginx
```

Make a note about the IP address from this container. It will be necessary for a further step.

10.Run the following command to access the container bash:

```
$ docker exec -it demo-1-nginx /bin/bash
```

11.Create a new HTML file on the container and exit:

```
root@e994cd7b1c5e:/# echo "This is my web nginx1." > /usr/share/nginx/html/adinusa.html
root@e994cd7b1c5e:/# exit
```

12.Using the IP address from step 8, try to access the previously created page:

```
$ curl IP:80/adinusa.html
```

The following output is be displayed:

```
 "This is my web nginx1."
```

13.It is possible to restart the container with the following command:

```
$ docker restart demo-1-nginx
```

14.When the container is restarted, the data is preserved. Verify the IP address from the restarted container and check that the adinusa page is still available:

```
$ docker inspect demo-1-nginx | grep IPAddress
```

```
$ curl IP:80/adinusa.html
```

15.Stop the HTTP container:

```
$ docker stop demo-1-nginx
```

16.Start a new HTTP container:

```
$ docker run --name demo-2-nginx -d nginx
```
17.Verify the IP address from the new container and check if the adinusa page is available:

```
$ docker inspect demo-2-nginx | grep IPAddress
```
```
$ curl IP:80/adinusa.html
```

The page is not available because this page was created just for the previous container. New containers will not have the page since the container image did not change.

18.In case of a freeze, it is possible to kill a container like any process. The following command will kill a container:

```
$ docker kill demo-2-nginx
```

This command kills the container with the SIGKILL signal. It is possible to specify the signal with the -s option.

19.Containers can be removed, discarding their state and filesystem. It is possible to remove a container by name or by its ID. Remove the demo-nginx container:

```
$ docker ps -a
$ docker rm demo-1-nginx
```


20.It is also possible to remove all containers at the same time. The -q option returns the list of container IDs and the docker rm accepts a list of IDs to remove all containers:

```
$ docker rm $(docker ps -aq)
```

21.Verify that all containers were removed:

```
$ docker ps -a
```

22.Clean up the images downloaded by running the following from a terminal window:

```
$ docker rmi nginx
```

**OR**

```
$ docker image rm nginx
```

This concludes the demonstration.