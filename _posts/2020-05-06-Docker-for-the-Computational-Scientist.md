---
layout: post
title: "Docker for the Computational Scientist"
date: 2020-05-06
---

Software technology changes at a very fast pace and computational scientists
profit greatly from the availability of open source platforms, code libraries,
public data banks and servers.
Interpreted languages like Python and R have gained rapid popularity in recent years,
because of the ease with which prototypes and publishable data and graphics can be created.

For CPU/GPU intensive work, compiled binaries from low-level languages like C have often
still the edge in terms of speed and memory load. Mixed high&low-level implementations
are not therefore not uncommon. The latter may lead to a tricky situation when it comes to
program dissemination. The computational scientist interested in a particular software package
needs to install various software packages, often accompanied with specifc version requirements
and poor backward compatibility. I guess almost every computational scientist has given up
on a software installation at one point in time because of compatibility problems.

Docker is a great solution for this type of dilemma.
A Docker image captures the essence of the computer environment of the program developer.
Copying that image to another (user's) computer and running it locally re-creates that environent in
a mini virtual machine. That means no additional installation is required,
the software runs with all the original dependencies in place.
An aditional bonus is that computational work becomes comparable between different scientists and groups,
as long as all participants use the same docker image, which come with a version tag.

Converting a software package into a Docker image is not difficult.
There are fundamentally two possible ways:

1. Using an existing (Docker image and adding software to it or
2. using a Docker file.

There are many descriptions how to do the above online, I will sketch below the way I created
a software package runnning a Shiny App to illustrate the process in a few lines.


