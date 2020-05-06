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
needs to install various software packages, often accompanied with specifc version requirements
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
There are fundamentally two possible ways: 1. By using an existing Docker image and adding software to it or
2. by creating a Docker file.

There are many detailed descriptions on the web about Docker image design.
Here I will just sketch the way I created a software package to run software on a Shiny App (via 1. method).
To use Docker one needs to learn few commands and the learning curve is steep.
Distributing Docker images is also easy: The Docker Hub is free to use for public images the same way GitHub is free for public software repositories.


