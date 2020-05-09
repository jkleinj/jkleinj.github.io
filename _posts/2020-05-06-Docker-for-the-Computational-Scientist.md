---
layout: post
title: "Docker for the Computational Scientist"
date: 2020-05-06
---

Software technology changes at a very fast pace and computational scientists
profit greatly from the availability of open source platforms, code libraries,
public data banks and servers.
Interpreted languages like Python and R have gained rapid popularity in recent years,
because of the ease with which prototypes, publishable data and graphics can be created.

For CPU/GPU intensive work, compiled binaries from low-level languages like C have
still the edge in terms of speed and memory load. Mixed high&low-level implementations
are therefore not uncommon. The latter may however lead to a tricky situation when it comes to
program dissemination. A computational scientist interested in a particular program most likely
needs to install various software packages, often accompanied with specific version requirements
and sometimes poor backward compatibility. I guess almost every computational scientist has struggled
with a software installation at least once because of compatibility problems.

Docker is a great solution for this type of dilemma.
A Docker image captures the essence of the computer environment of the program developer.
Downloading that Docker image to a user's computer and running it locally re-creates the developer's
environment in a type of minituarised and lightweight virtual machine.
That means no additional installation is required, the software runs with all the original dependencies in place.
An aditional bonus is that computational work becomes comparable between different scientists and groups,
as long as all participants use the same docker image, which come with a version tag.

Converting a software package into a Docker image is not difficult.
There are fundamentally two possible ways: 1. By using an existing Docker image and adding software to it or 2. by creating a Docker file.

There are many detailed descriptions on the web about Docker image design.
Here I will just sketch the way I created a software package to run our software
[POPScomp](http://popscomp.org:3838) as a Shiny App (*via* method 1).
To use Docker one needs to learn few commands and the learning curve is steep.
Distributing Docker images is also easy: The [Docker Hub](https://hub.docker.com/)
is free to use for public Docker images the same way GitHub is free for public software repositories.

## Preliminaries
A Docker **image**, when running on the local computer, initialises a **container**.
The commands below are run on the terminal shell of a computer with [Docker installed](https://docs.docker.com/).
Output is commented out with a hash.
Docker runs with root permissions, therefore you either need to become 'root' user or prepend the following commands with 'sudo'.

## Download RStudio Docker Image
Because we want to use a Shiny App, the easiest starting point is an image containing RStudio.
```
docker pull rocker/rstudio
```
We can list the downloaded image(s).
```
docker images
#REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
#rocker/rstudio       latest              035af5ff999f        2 weeks ago         1.46GB
```

## Initialise a Container
In the next command we will initialise a container *via* the 'IMAGE ID' and additionally specify a 'USERID', 'PASSWORD' and port (-p) for RStudio (and Shiny). RStudio is automatically started in the container and accessible in a browser under http://localhost:8787. Do not stop this process until the modified container has been committed, see below.
```
docker run -e USERID=1000 -e PASSWORD=<myPassword> --rm -p 8787:8787  035af5ff999f
```
Like the image, the container has a specific ID as shown by the following command. You might need to open a second (root) shell if the 'docker run' command above has not returned the prompt.
```
docker ps
#CONTAINER ID    IMAGE           COMMAND     CREATED          STATUS          PORTS                     NAMES
#911a29f6d77b    035af5ff999f    "/init"     2 minutes ago    Up 2 minutes    0.0.0.0:8787->8787/tcp    elated_napier
```

## Modify the Docker Image
The initialised container can be accessed like a virtual machine. Note that we use the container ID in those commands.
For example, open a bash shell in the container:
```
docker exec -it 911a29f6d77b bash
```
Or copy code to the container:
```
docker cp ~/mySoftware.tgz 911a29f6d77b:/root/mySoftware.tgz
```
That way one can perform the usual software installations, including software updates on the operating system and the R environment, for example.

## Commit the Modified Container to a New Image
After performing all the required modifications (software installations) on the running container, the modified container is committed *via* its ID to a new image. That new Docker image will contain the software of the modified container.
```
docker container commit 911a29f6d77b <owner>/<repository>:<x.y.z>
```
This command commits the container locally, where \<owner\> and \<repository\> should be valid names and the tag \<x.y.z\> is usually a 3-digit version number. Command line examples can be found [here](https://docs.docker.com/engine/reference/commandline/commit/). After committing the modified container, the 'docker run' process that has been started at the beginning may now be terminated.
To upload the image to Docker Hub, use the login and push commands.
```
docker login
docker push <owner>/<repository>:<x.y.z>
```

## Summary
We have created a Docker image containing our own software release in only 9 'docker' command lines.
Docker Hub holds a wide range of base images to start with and other computational scientists will appreciate your software in Docker images as easily accessible and platform-agnostic releases.


