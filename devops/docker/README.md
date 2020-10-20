# Docker

[back](../README.md)

Docker is a platform for developers and sysadmins to **build**, **run** and **share** applications with containers.

The use of containers to deploy applications is called *containerization*.

## Hello world

After install Docker it is possible execute a `hello-world`.

``` sh
$ docker run hello-world
```

The command above downloads an image and run it in a container. The container shows a `Hello from Docker!` message. After the execution the container finish its execution.

// TODO: Other Docker objects.

## Images

An Image is a read-only template with instructions for creating a Docker container.

To build a image. It should create a `Dockerfile` with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a `Dockerfile`  creates a layer in the image. When a change was made into `Dockerfile` and rebuild the image, only those layers which have changed are rebuilt.



## Alpine

There is a minimal Docker image based on Alpine Linux with a complete package index and only 5MB in size.

``` sh
# It is possible execute commands into the container

$ docker run alpine:3.12 ls -l

# It shows a list of the directories at the container.

# Also it is possible execute a shell into the container.

$ docker run -it alpine:3.12 sh

# The flag -it is necessary to create a interactive process.
```


``` sh
# Download an ubuntu image and create a container.
$ docker run -it ubuntu /bin/bash

$ apt-get update
$ apt-get install figlet

# Show latest ten containers including the finished.
$ docker ps -a | head

# Create an image from a container
$ docker commit <ID-CONTAINER>

# Show ten last images
$ docker image ls | head

# Add tag to image.
$ docker image tag <ID-CONTAINER> <TAG>:<VERSION>

# Show images
$ docker image ls | head

# Try the new image
$ docker run <TAG> figlet "Hello World"
```
Create a `Dockerfile` into the file add the next:

``` Dockerfile
# Always start from an existing image
FROM ubuntu

# Execute the commands
RUN apt-get update && apt-get install figlet -y
```
Then it is possible create a docker image

``` sh
# Build image
$ docker build -t <TAG>:<VERSION> <Dockerfile-PATH>
# Run image
$ docker run <TAG>:<VERSION> figlet <SOME-TEXT>
# Show history in a image
$ docker image history <ID/TAG>
```

Creating a Nginx container

``` bash
# creating an image
# with detach to stay running the container in background.
$ docker run -d nginx:1.19.3

  # Updating the packages and installing the procps, curl packages.
  $ apt-get update && apt-get install -y procps curl

  # Showing the process into the container
  $ ps fax

  # Showing the content of nginx
  $ curl localhost

  # Stopping container
  $ docker stop <ID>

# Create a index.html file
# initialization of the Nginx container with a volume tho load data and exporting ports to access to container from outside of it.
$ docker run -v ~/app-nginx/index.html:/usr/share/nginx/html/index.html:ro -p 8080:80 -d nginx:1.19.3

# View data into the container
$ curl localhost:8080

# Any time that a change occur into index.html file will be mirrored into the container.
```
# Resources

[Docker](https://docs.docker.com/get-started/)

[Alpine](https://hub.docker.com/_/alpine)