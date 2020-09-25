<!-- TOC -->

- [Docker Notes](#docker-notes)
    - [Docker Concepts](#docker-concepts)
        - [Images and Containers](#images-and-containers)
            - [Export/Import, Save/Load and Commit Images](#exportimport-saveload-and-commit-images)
            - [Image & Container Layers](#image--container-layers)
            - [Docker storage driver](#docker-storage-driver)
        - [`bindmount` to access host file system](#bindmount-to-access-host-file-system)
        - [Container vs. Virtual Machine](#container-vs-virtual-machine)
    - [Examples](#examples)
        - [More `run` examples](#more-run-examples)
        - [Build images](#build-images)
        - [Windows Container](#windows-container)
- [Deep Learning with Docker](#deep-learning-with-docker)

<!-- /TOC -->

# Docker Notes

[anaconda docker images](https://medium.com/@patrickmichelberger/getting-started-with-anaconda-docker-b50a2c482139)

## Docker Concepts

[Docker Terms, e.g., Image, Container, and etc. ](https://towardsdatascience.com/how-docker-can-help-you-become-a-more-effective-data-scientist-7fc048ef91d5): for **data scientists**.

​	- Docker daemon and client

### Images and Containers

- **Base images**: have no parent image, usually images with an OS like ubuntu, busybox or debian 
- **Child images**: build on base images and add additional functionality 

#### Export/Import, Save/Load and Commit Images
[See explanation](https://sysadminnightmare.org/docker-save-load-import-export-commit/)

`docker commit` to commit changes and save a new image. See [example](https://blog.codeship.com/using-docker-commit-to-create-and-change-an-image/)
```sh
# Save a container as a new image
docker commit <DOCKER_ID>  YOUROWN_DOCKER_IMAGE_NAME
```

#### Image & Container Layers
[See explanation](https://medium.com/@jessgreb01/digging-into-docker-layers-c22f948ed612)
- When Docker builds the container from a *Dockerfile*, each step corresponds to a command run in the Dockerfile. 
- Each layer is made up of the file generated from running that command. 
- A container has a *writeable* layer that stacks on top of the image layers. This writeable layer allows you to “make changes” to the container 

Use `docker history <image_name>` to view image layers

#### Docker storage driver

It uses a **Union Mount** system to implement image layers. Union mount is a way of combining numerous directories into one directory that looks like it contains the content from all the them.
>- merge all the files for each image layer together and presents them as one single read-only directory at the _union mount point_.
>- Each **image layer** is called a branch

This is illustrated below.
![alt](https://docs.docker.com/storage/storagedriver/images/aufs_layers.jpg) 

### Host-Container Data Sharing 
`bindmount` to access host file system

[see here](https://www.digitalocean.com/community/tutorials/how-to-share-data-between-the-docker-container-and-the-host)

```shell
# binmount volumn
-v ~/nginxlogs:/var/log/nginx
```

*docker cp*
```shell
docker cp index.html <container_name>:/container/path
```

### Container vs. Virtual Machine
[Illustrations](https://nickjanetakis.com/blog/comparing-virtual-machines-vs-docker-containers)

Virtual machines have a full OS with its own memory management installed with the associated overhead of virtual device drivers. In a virtual machine, valuable resources are emulated for the guest OS and hypervisor, which makes it possible to run many instances of one or more operating systems in parallel on a single machine (or host). 

On the other hand Docker containers are executed with the Docker engine rather than the hypervisor. Containers are therefore smaller than Virtual Machines and enable faster start up with better performance, less isolation and greater compatibility possible due to sharing of the host’s kernel.

There is one key metric where Docker Containers are weaker than Virtual Machines, and that’s “**Isolation**”. Intel’s VT-d and VT- x technologies have provided Virtual Machines with ring-1 hardware isolation of which, it takes full advantage. It helps Virtual Machines from breaking down and interfering with each other. Docker Containers yet don’t have any hardware isolation, thus making them receptive to exploits.

## Examples

```shell
# docker --help
# docker <cmd> --help

# pull an image fro docker registry (hub): "docker search" to search for images
docker pull busybox
docker pull ubuntu:14.04  # :<tag> to specify a version
docker pull python:2.7 # see https://hub.docker.com/_/python/

# list installed images
docker image
docker rmi  # remove images

# run container
docker run --name=<name> -it ubuntu bash  # -it == interactive tty
docker run --rm -it hello-world # --rm :  auto delete container once it's exited

# show all running containers
docker ps # switch -a
# output with selected fields
# see https://theforgetful.dev/posts/docker-ps-output-too-wide/
alias dps='docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"'

# clean up containers that are no longer needed to save disk space
docker rm <container_id>  # use "docker ps -a" to find container ID
# -q: only return numeric ID
# -f: filter
docker rm $(docker ps -a -q -f status=exited)  
```
### More `run` examples

```shell
# -d: detach-terminal mode
# -P: publish all exposed ports to random ports
# -p <to-port>:<from-port>  map 'from-port' to 'to-port>
docker run -d -P --name <container_ID> <image> 
docker port <container_ID> # show port mapping

# run a command in a running container
docker exec -it CONTAINER_ID bash

docker stop <container_ID> # stop a detached container
# restart an exited container
docker start  `docker ps -q -l` # restart it in the background
docker attach `docker ps -q -l` # reattach the terminal & stdin
# Ctrl+p, Ctrl-q to dettach a container (i.e., transition from interactive mode to daemon mode)
```

### Build images

`Dockerfile`

```dockerfile
# our base image
FROM python:3-onbuild  # onbuild version auto copy files and install dependencies

# specify the port number the container should expose
EXPOSE 5000

# run the application
CMD ["python", "./app.py"]
```

```shell
docker build -t <tag-name> # take Dockerfile as input, -t: tag name
```





### Windows Container

[Windows container instruction by MS](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-10#3-install-base-container-images): 


# Deep Learning with Docker
Use nvidia-docker to enable GPU in container
> [nvidia docker plugin?](https://github.com/NVIDIA/nvidia-docker/wiki/nvidia-docker-plugin)

[More details at hackernoon](https://hackernoon.com/docker-compose-gpu-tensorflow-%EF%B8%8F-a0e2011d36)

[nVidia docker images](https://hub.docker.com/r/nvidia/cuda/)
- For Centos: https://hub.docker.com/r/nvidia/cuda/tags?page=1&name=centos7

From [nvidia-docker github wiki](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#i-have-multiple-gpu-devices-how-can-i-isolate-them-between-my-containers)
```sh
# If you have 4 GPUs, to isolate GPUs 3 and 4 (/dev/nvidia2 and /dev/nvidia3)
$ docker run --runtime=nvidia -e NVIDIA_VISIBLE_DEVICES=2,3 nvidia/cuda:10.0-base nvidia-smi
```
