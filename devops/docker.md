# Docker

## Container

- A way to package application with all the necessary dependencies and configuration
- Portable artifact, easily shared and moved around
- Makes development and deployment more efficient

### Docker Image vs Container

Image is the actual package. It is the artifact which can be moved around. When the image is run, container environment is created. Container is a running environment for image.

### Docker vs VM

VMs consist of two layers - OS kernel and application layer. However, Docker only consists of the application layer.

### Docker Commands

- **docker pull \<image>** - to pull an image from DockerHub
- **docker images** - list of images pulled
- **docker run \<image>** - start image in container, we can use the **--name** flag to set a custom name for the container
- **docker run -d \<image>** - start image in container in detatched mode
- **docker run -p6000:6379 \<image>** - to specify the port on the local machine
- **docker ps** - list running containers
- **docker stop \<contai\*ner_id>** - to stop a container
- **docker start \<container_id>** - to start a container
- **docker ps -a** - list running and stopped containers
- **docker logs \<container_id>** - check logs of a container
- **docker exec -it \<container_id> /bin/bash** - to navigate the file system of the container

![Docker Workflow](../images/docker-workflow.png)

### Docker Compose

Running multiple services separately can be tedious. Using Docker compose, we can specify all the services and their configurations in a single file.

For example, for the following _docker run_ command -

```
docker run -d \
--name mongodb \
-p 27017:27017 \
-e MONGO-INITDB_ROOT_USERNAME=admin \
-e MONGO-INITDB_ROOT_PASSWORD=password \
--net mongo-network \
mongo
```

we can specify the following **\*mongo-docker-compose.yaml** file -

```
version: '3' //version of docker compose
services:
    mongodb:
        image: mongo
        ports:
         - 27017:27017
        environment:
         - MONGO-INITDB_ROOT_USERNAME=admin
         - MONGO-INITDB_ROOT_PASSWORD=password
```

We can add more services in a similar way.
We don't need to define network in docker compose because it takes care of creating a common network.

Command to start:

> docker-compose -f <yaml_file> up

Command to stop:

> docker-compose -f <yaml_file> down
