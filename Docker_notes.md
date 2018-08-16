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