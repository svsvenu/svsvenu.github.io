---
layout: post
title: Deploy a micro-service on native docker and openshift
{% include disqus.html %}

---

## Assumptions

This blog assumes that you have played around with docker and openshift and have deployed and image
or two along the way on both the platforms. A few things to ensure before you start running this
applicaton are

- Docker daemon is running ( In my case i am running docker for mac )
- Openshift is running ( I am using minishift but you could potentially use the real version )
- oc tool ( The client for accessing openshift ) is in the path and can be accessed from any terminal window


## Introduction

The application is a simple rest end point hosted by camel that uses netty as its http layer. It returns
the string "Hello world" when invoked. Notice that its not a applicaton that is "deployed" on a server
in fact it comes bundled with an http layer, a la spring boot. The maven file for this project has three plugins
that 

- Create a runnable jar
- Creates a docker image and pushes it on to docker hub
- Pulls down the docker image from docker hub and pushes it to open shift and creates a route

## The bundler

The very first step in the process is to build a jar file that is "runnable", The application itself is nothing more
than a camel route, and a blocking camel main to keep it running. We are using the maven-assembly-plugin to
create a jar with all the dependencies bundled up

## The docker plugin

The fabric8 docker plugin ties the goal of docker image creation to the build phase of install and creates a tar
ball of the image. The additional goals of push and start push the newly created image up to docker hub and also
start the image locally

## Open shift commands

The image that has been pushed up to docker hub is now used by open shift to deploy. The open shifts commands
are located in oc-deploy.sh. A brief explanation below
```
oc login -u=admin -p=admin
```
The login command tries to login to the open shift server running locally. The default connection settings
are saved in ~/.kube/config file. 

```
oc delete all --all
```
This command removes everything ( services, routes etc ) that have been added via the new-app command

```
oc new-app svsvenu/fabric-camel-rest:1.0-SNAPSHOT
```
Creates s new-app by downloading the artifact from docker hub
```
oc expose svc/fabric-camel-rest --hostname=fabric-camel-rest-my-project.192.168.64.3.nip.io
```
Creates a route to access the end point independent of the number of containers running in the 
background
```
oc logout
```
Yea, does what it says

