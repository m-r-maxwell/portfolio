+++ 
draft = false
date = 2024-05-16T18:11:44-04:00
title = "Docker"
description = ""
slug = ""
authors = []
tags = ["docker", "kubernetes"]
categories = []
externalLink = ""
series = ["Dev tools"]
+++

### Containers Vs Virtual Machines
- Virtual Machines virtualize hardware
- Containers virtualize OS kernels

### Anatomy of a Container
- Linux namespace
- Linux control group

Namespaces provide different views of your system and are a linux kernel feature. 
Provided namespaces:
| Name | Description |
| :---: | :--- |
| USERNS | User Lists |
| MOUNT | Access to file systers|
| NET | Network communication |
| IPC | Interprocess communication |
| TIME | The ability to change time |
| PID | Process ID Management |
| CGROUP | Create control groups|
| UTS | Create host/domain names|
<br />

*Important note:
Docker containers use every namespace but time and you cannot change time.

### Control Group Uses
1. Monitor and restrict CPU Usage (e.g. CPU throttling)
2. Monitor and restrict network and disk bandwidth
3. Monitor and restrict memory consumption (e.g. memory quatas)

### Docker Limitations
- Primarily runs on Linux, although containerization options exist for Windows Server and Docker Desktop offers Windows containers

### Docker Advantages
1. Dockerfile
    - Makes configuring and packaging apps and their environments really easy.
2. Docker Hub
    - Makes sharing images with anyone in the world easy.
3. Docker CLI
    - Makes it easy to start your apps in containers (Alternatively, using Docker Desktop)


**_There was a licensing change in 2021 for Docker Desktop_**
- Companies with more than 250 employees and $10 million in revenue are required to purchase subscriptions before using

Alternatives options to Docker Desktop
- Colima
- Rancher Desktop
- Podman


## Working with Docker
- docker container create and start 
- docker run

### Special Notes
Volumes allow persistenting data outside of the container, ensuring data isn't lost when containers are recreated

Docker Networking enables you to connect containers together and manage their communication

### Container Create/Start
```shell
#:linux is the image tag
docker container create hello-world:linux
Unable to find image 'hello-world:linux' locally
linux: Pulling from library/hello-world
478afc919002: Pull complete
Digest: sha256:b7d87b72c676fe7b704572ebdfdf080f112f7a4c68fb77055d475e42ebc3686f
Status: Downloaded newer image for hello-world:linux
c0750dcfc4ddb4114079cce210503d3d683adf820d913cefe49830befb04db3a
```
This creates the container but it does not start them. We can verify that it is available with: `docker ps --all`

To start the container let's run
```shell
docker container start c07 #using short id to run
```

We can verify that the container ran by running: 
```shell
docker logs c07

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

If we do not want to have to check logs, we can attach our terminal to the container with `docker container start --attach <id>`

### Docker Run
This is a much simpiler way to run and get output from a container.
`docker run hello-world:linux`
- This creates a container from the image
- Starts the container
- Attaches the output to show immediately

### Let's Create Our Own Container
Dockerfile:
```dockerfile
FROM ubuntu
USER root
COPY ./entrypoint.bash /
RUN apt -y update
RUN apt -y install curl bash
RUN chmod 775 /entrypoint.bash
USER nobody
ENTRYPOINT ["/entrypoint.bash"]
```

Entrypoint.bash:
```bash
#!/usr/bin/env bash

bash_is_current_version() {
  bash --version | grep -q 'version 5'
}
curl_is_installed() {
  &>/dev/null which curl &&
    curl --version | grep -q '^curl'
}
get_current_date() {
  curl --silent --insecure -I google.com |
    grep -E '^Date' |
    sed 's/^Date: //' 
      xargs -I {} date --date='{}'
}
if ! bash_is_current_version
then
  >&2 echo "ERROR: Bash not installed or not the right version."
  exit 1
fi
if ! curl_is_installed
then
  >&2 echo "ERROR: Curl is not installed."
  exit 1
fi
echo "Hello! The current date and time is $(get_current_date)"
```

```bash
#the period is for where the dockerfile is, . is for our current directory`
docker build -t our-first-image . 
#output
#a good chunk has been removed from installing
[+] Building 24.4s (10/10) FINISHED                        
 => => writing image sha256:52efcd8ea3f2b2e528fe245e9ded3d394aab0f650a3f7  0.0s
 => => naming to docker.io/library/out-first-image
#now let's run a container from the image
docker run out-first-image
Hello! The current date and time is Thu, 16 May 2024 13:59:01 GMT
```

But let's say you want to run an container as a server with the following:
`docker run our-server`. This causes a problem since we can't really interact with the server this way instead we should do `docker run -d our-server` to let the container run but without being attached to the terminal. We can interact with the container using exec: `docker exec <id> cmd` but we can go further in interacting with the containers terminal using `docker exec --interactive --tty <id> bash # ctrl+D to exit`

### Stopping or Removing a container
- Stopping a container
    - `docker stop <id>` Graceful shutdown
    - `docker stop -t 0 <id>` Force shutdown
- Removing a container
    - `docker rm <id>`
    - `docker rm -f <id>` Stops a running container and removes it
    - `docker ps -aq | xargs docker rm` List all container ids with q and pipe that to xargs to loop over each container and remove them

### Removing Images
- `docker rmi <tag>`
- `docker rmi -f <tag>` Force the removal

### Other things
- `--name` can name a container to use in commands
- `-p 5001:5000` port binding, first number is for our computer, the second number is for in the container
- `-v /path/to/mount:/container` mounting output from container to our machine and sometimes you might need to add quotes around the paths
- `--rm` can be added along with docker run to delete the container
- `docker system prune` this will clean up unused docker related items from your system
- `docker stats <id> OR <name>` to see stats about the container
- `docker top` - sorted information about processes in the container
- `docker inspect <id> or <name> | less` - even more information about the container

### Docker Compose
Docker compose is a part of the docker CLI that is designed to stand up and connect multiple containers with the command `docker-compose up`. This command looks for a yaml file that contains information to run multiple images together.<br />
Ex:
```yml
services:
    web:
        build: .
        ports:
            - "8080:80"
    db:
        imgage: "whatever-db-you-use
```

### Best Practices
- Use verified images
- Use Container Image scanning
- Avoid using latest as a tag
- Use non-root users

The last part is container organization which you can learn more about [here](../kubernetes).