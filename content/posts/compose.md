+++ 
draft = false
date = 2024-05-17T10:39:34-04:00
title = "Docker Compose"
description = "Docker compose information and use"
slug = ""
authors = []
tags = ["Dev Tools", "Docker"]
categories = []
externalLink = ""
series = ["Docker"]
+++


### What is Docker Compose
Essentially, a markdown tool for specify docker configurations as code, designed to run in non production environments.

### Where To Use Docker Compose
- Staging and Local Development

Docker Compose is not recommended for use in production environments and instead something like Kubernetes or Docker Swarm is recommended.

### Configuration Examples
- Where persistent data lives
- How to access or send messages to other services
- Environment specifc values to use

### Steps

|Procedural | Declarative|
| --- | --- |
| Series of ordered steps | Specify end results |
| Based on assumptions about previous steps | System will determine which steps to execute |
| Easy to introduce errors | Produces the same results each time|

### Main Benefits
- Version control (Adding docker-compose.yml to SVC)
- Self documenting
- Easy management of docker environments through individual config files

### Where to use Docker Compose

| Designed For | Not Designed For|
| --- | --- |
| Local Development | Distrubuted Systems |
| Staging Server | Complex production environments|
| Continuous integration testing environments | |

*Special Note:
Docker Compose does not have tooling for running containers across multiple hosts or any kind of auto scaling. Meaning if you wanted to scale your applications you would need to run docker-compose on multiple hosts.

### Writing a Docker Compose Config
```yml
version: "3.9"
service: # what we want to run
    frontend: # service names are meant to be human readable
        build: /path/to/imagefile #or . for current directory.
    database:
        image: "postgresql" #fetch from docker hub
```

### Most Common Docker Compose Commands
- docker-compose up
- docker-compose down
- docker-compose stop
- docker-compose restart

docker-compose up:
This will build all of the images under the service name in the yml file and run them. You can also provide the name for whatever image you want to specifically work with, ex: `docker-compose up frontend`

docker-compose down:
Stops all containers, deleting all containers and any produced artifacts

docker-compose stop:
Useful for running locally and saving battery life and freeing up memory by halting the containers.

docker-compose restart:
Shorthand for `docker-compose stop; docker-compose start`. The equivalent of turning it off and on again.

### Build Arguments and Environment Variables
We can add build arguments inside of the yml file if we need to provide additional information during build time, such as regions.

```yml
version: "3.9"
service: 
    frontend:
        build:
            context: /path/to/imagefile
            args: 
                - region=east-1
                - name=0 #build arguments can have any value
        environment: 
        #non value env variables will try to pull from your local
        #export runtime_env=dev
            - runtime_env 
    database:
        image: "postgresql"
        #or we can pass in env files
        env_file:
            - ./postgres/env
```

### Volumes
Adding a mounted volume can be done with the following. Pathing is based on bash pathing.
- ./ is the current directory
- ../ is the previous directory
- / is root directory

You can also add `:rw` at the end of the volume path to allow read-write. If you only need to read persistent data you can add `:ro`.

We can aslo use named volumes
```yml
version: "3.9"
service: 
    frontend:
        build:
            context: /path/to/imagefile
            args: 
                - region=east-1
        environment: 
            - runtime_env=dev
    database:
        image: "postgresql"
        env_file:
            - ./postgres/env
        volumes:
            # if you want to add a custom source location
            # full exampe is source:target:access mode
            - ./postgres:/path/to/directory:rw
            # below is letting docker pick the source
            #- /path/to/directory
# named volumes
volumes:
    #name goes here
    post: #this would change the above volume to 
    # post:./postgres:/path/to/directory:rw
```

If we want to take down only the volumes we can run `docker-compose down --volumes` allowing us to reclaim memory back. If we combine this with namewd volumes, we end up circumventing the problem of letting docker create volumes all over the place taking up storage.

The above example uses the short syntax for delcaring volumes, here is the long syntax as well.
```yml
type: volume
source: post
target: /path/to/directory
read_only: false
# compared to short
post: ./postges:/path/to/directory:rw
```

### Ports
We can map ports from the containers to our local machine, allowing us to interact with the containers or allow for the containers to interact with each other.
The syntax is the same from regular docker files: From:To.
```yml
version: "3.9"
service: 
    frontend:
        build:
            context: /path/to/imagefile
            args: 
                - region=east-1
        environment: 
            - runtime_env=dev
        ports: 
            - "80:80"
            # if we want to expose another port for something like metrics we can also do that
            - "443:443"
    database:
        image: "postgresql"
        env_file:
            - ./postgres/env
        volumes:
            - post:/path/to/directory:rw
        ports:
            - "5432:5432"
volumes:
    post:
```

### Start Up Order
Since some services can require another service to be able to run properly, such as a backend needing to connect to a database. We can use the depends_on flag inside of the yml file.

```yml
version: "3.9"
service: 
    backend:
        build: .
        depends_on:
            - database
    database:
        image: "postgresql"
```

if we want to start a service that as any dependancies with `docker-compose up backend`, docker-compose will start up those first and continue on. However, this does not guarantee that the dependancies are healthy and available, just that they are started first.

### Named Subsets of Service or Profiles

```yml
version: "3.9"
service: 
    backend:
        build: .
        depends_on:
            - database
        profiles:
            - backend_service
    frontend:
        build: ./frontend
        profiles:
            - frontend_service
    database:
        image: "postgresql"
        # leaving out a profile ensures that this is in the default profile and gets added to other profiles
```

Now we can run profiles such as `docker-compose --profile backend up`
allowing us to run just the services needed for the backend service.

### Multiple Docker Compose Files
We can also use an override file for different environments.
- `docker-compose.local.yml` <- local settings
- `docker-compose.staging.yml` <- staging settings

In this case, the list values in the main docker-compose file will have the override files appended to the list. If it is a single field value, the override value will be used, such as in build paths.

We can run override files with the following: `docker-compose -f [primary file] -f [override file] [command]`

We can tie this together with environment variables in a .env file.
```
TAG=latest
``` 
which can be used in the yml file.
```yml
version: "3.9"
service: 
    backend:
        build: .
        depends_on:
            - database
        profiles:
            - backend_service
    frontend:
        build: ./frontend
        profiles:
            - frontend_service
    database:
        image: "postgresql:${TAG}"
```

If our env file is located else where we can pass it via the cli with `docker-compose --env-file [path]`
The last important piece to know is any env variable set in the host shell is the value that will be used.

### More Information
- To learn more about kubernetes please see [here](../kubernetes)