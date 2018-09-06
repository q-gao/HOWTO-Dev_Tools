

# Docker Notes

[anaconda docker images](https://medium.com/@patrickmichelberger/getting-started-with-anaconda-docker-b50a2c482139)

## Docker Concepts

[Docker Terms, e.g., Image, Container, and etc. ](https://towardsdatascience.com/how-docker-can-help-you-become-a-more-effective-data-scientist-7fc048ef91d5): for **data scientists**.

​	- Docker daemon and client

### Images

- **Base images**: have no parent image, usually images with an OS like ubuntu, busybox or debian 
- **Child images**: build on base images and add additional functionality 

`docker commit` to commit changes and save a new image. See [example](https://blog.codeship.com/using-docker-commit-to-create-and-change-an-image/)

### `bindmount` to access host file system

[see here](https://www.digitalocean.com/community/tutorials/how-to-share-data-between-the-docker-container-and-the-host)

```shell
-v ~/nginxlogs:/var/log/nginx
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
docker run -it ubuntu bash  # -it == interactive tty
docker run --rm -it hello-world # --rm :  auto delete container once it's exited

# show all running containers
docker ps # switch -a

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