# Docker

Docker is a software tool that manages the creation, build and run of containers. 
All containers are run by single operating system kernel and therefore use fewer resources than virtual machines.

## Manage data in Docker

By default the data doesn't persists when the container no longer exists.
Docker has two options for containers to store files in the host machine, so that files are persisted even after the container stops: *volumes* and *binds*.

* _Volumes_ are stored in a part of the host filesystem which is *managed by docker*. Volumes are the best way to persists data in docker.
* _Bind mounts_ may be stored *anywhere* on the host system. They may even be important system files or directories.
* _tmpfs_ mounts are stored un the host system's memory only, and are never written to the host's system's filesystem.

[manage data docker](https://docs.docker.com/storage/)

## Post install

https://docs.docker.com/engine/install/linux-postinstall/

sudo usermod -aG docker $USER

## Network

[docker network](https://docs.docker.com/network/)

## Dockerfile

The dockerfile contains the instructions to create docker images.
The image can be created using docker build.

Example of dockerfile that install curl

```bash
FROM alpine
RUN apt-get update -y && apt-get -y install curl
```

[docker builder](https://docs.docker.com/engine/reference/builder/)


Differences between RUN,CMD and ENTRYPOINT

### RUN

Executes commands in a new layer and creates new image. E.g., it is often 
used for installing new softwar (apt, npm, etc).

### CMD

Sets default command and/or parameters, which can be overwritten from command line when
docker container runs.

#### ENTRYPOINT

configures a container that will run as an executable

## Docker Compose

Compose is a tool for defining and running multi-container Docker applications.
Using Compose is basically a three step process:
1. Create a *Dockerfile*.
2. Create the services,envs, volumes inside *docker-compose.yml*.
3. Run *docker-compose* up

[https://docs.docker.com/compose/](https://docs.docker.com/compose/)

## Commands

tags and versions                     --> docker run ... <NAME>:<VERSION>
List local images                     --> docker image ls
List running containers               --> docker ps
Run container with console            --> docker -it run <DOCKER_ID> bash
Builds a image from dockerfile        --> docker build .
Creates volume to share/persists data --> docker run -v <HOST FS>:<CONTAINER FS> ...
Expose ports                          --> docker run -p <HOST PORT>:<CONTAINER PORT>
env variables                         --> docker run -e USERNAME=andres ..
env file                              --> docker run --env-file env
