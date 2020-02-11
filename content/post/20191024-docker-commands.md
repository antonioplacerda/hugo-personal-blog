---
title: "Docker Commands"
date: 2019-10-24T22:24:31+01:00
draft: false
css: []
tags: ["docker", "tips"]
---

This is a list of every docker command I find useful and a bit of info about it. It is not intended to be a tutorial of any kind, but maybe someone will find this list as useful as I consider it to be!

General
-------

- **docker version** - if any error here, docker is probably not running. If on linux, do a sudo
- **docker info** - a lot more info about the docker installation

Containers
----------

- **docker container run IMAGE** - first it gets an image from local machine, if it doesn’t find one, tries to get one from docker hub. If not connected to the internet and one does not have the image locally, well you should not live in the cave ages. The docker container run always ends with the image name if no command is passed. The version of the image can be specified here, using image:version. It defaults to image:latest. It will give it a virtual IP on a private network, named bridge, inside docker engine. It will open up port 80 on host and will forward all traffic to port 80 in the container.
- **docker container run --publish,-p xx:yy IMAGE** - it redirects routing from the port xx of your host to the port yy of the container
- **docker container run --detach,-d IMAGE** - keeps the container running detaching it from the terminal session
- **docker container run --name CONTAINER IMAGE** - creates a container with the specified container_name
- **docker container run --rm IMAGE** - runs a container and deletes it afterwards
- **docker container run --env,-e ENV_LIST IMAGE** - sets environment variables to pass to the container
- **docker container run --network NETWORK IMAGE** - it will create the container on the specified network. It will default to the network bridge
- **docker container run --volume,-v NAMED_VOLUME:VOLUME IMAGE** - it will bind mount a volume with the specified name. if the named volume start with a forward slash, it will map to the specified path on the host. if $(pwd) is used, it will map to the current working directory

---

- **docker container run -it CONTAINER bash** - simulates a real terminal to interact with the container. This way, when we exit the bash, the container will stop because the container only runs as long as the command that it ran on startup runs.
- **docker container exec -it CONTAINER bash** - simulates a real terminal to interact with the running container. This way, the container will keep running once this process is exited
- **docker container stop CONTAINER \[CONTAINER...\]** - stops a running container

---

- **docker container ls (docker ps)** - lists all the running containers
- **docker container ls -a (docker ps -a)** - lists all existing containers, even the non-running ones
- **docker container rm CONTAINER \[CONTAINER...\]** - deletes the specified containers. Here, the full uid can be passed or the first 3 letters of the uid since it’s all it takes for it to be unique, or the container name. If no name was specified, it can be found doing a docker container ls -a. A running container cannot be deleted this way.
- **docker container rm -f,--force CONTAINER \[CONTAINER...\]** - Since a running container cannot be deleted the usual way, the force option must be passed
- **docker container logs CONTAINER** - see the logs of the container
- **docker container logs -f CONTAINER** - see the logs of the container and keeps following it. ctrl+c to exit
- **docker container top CONTAINER** - display the running processes of a container
- **docker container inspect CONTAINER \[CONTAINER...\]** - shows detailed information of the containers specified. It doesn't tell anything about the active container and what it's doing
- **docker container stats** - shows a streaming view of live performance data about the running containers

---

- **docker container port CONTAINER** - checks the routing from the host to the container

Network
-------

- **docker network ls** - The network bridge, docker0 on older versions, is the default network that bridges through the NAT firewall to the physical network the host is connected to. By default, all containers will be in this network. The network named host is a special network that skips the virtual networking of docker and attaches the container directly to the host interface. This way, it gain performance but sacrifices security of the container model. The network named none is the equivalent of having an interface on your computer that is not attached to anything
- **docker network inspect NETWORK \[NETWORK...\]** - inspects the specified networks, displaying detailed information on them. This is how to find out which container are in which network
- **docker network create NETWORK** - spawns a new network and allows to attach containers to it
- **docker network connect NETWORK CONTAINER** - connects the specified container to the specified network. A container may be connected to multiple networks
- **docker network disconnect NETWORK CONTAINER** - disconnects the specified container from the specified network

Images
------

- **docker image ls** - lists all the images in the host
- **docker image rm  IMAGE \[IMAGE...\]** - deletes the specified images. The full uid can be passed, or only the first 3 characters or the repository:tag
- **docker image tag SOURCE_IMAGE\[:TAG\] TARGET_IMAGE\[:TAG\]** - It defaults to latest if no tag is passed. The best name would be default instead of latest since latest is not always the latest version of the image
- **docker image push IMAGE** - pushes the image to docker hub
- **docker image build -t,--tag TAG PATH | URL** - builds an image with the specified tags, latest if none is supplied. A path to a dockerfile must be supplied

Volumes
-------

- **docker volume ls** - lists all existing volumes
- **docker volume rm VOLUME \[VOLUME...\]** - removes the specified volume
- **docker volume prune** - removes all non used volumes
- **docker volume inspect VOLUME \[VOLUME...\]** - the mount point is not accessible directly from the host if using a mac, since docker creates a tiny linux vm to run, so all this data will be stored there. there is a way to get there, but I haven't figured it out yet
- **docker volume create VOLUME** -
