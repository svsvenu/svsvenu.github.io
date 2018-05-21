---
layout: post
title: Deploy a microservice on native docker and openshift
---


## Assumptions

This blog assumes that you have played around with docker and openshift and have deployed an image
or two along the way on both the platforms. A few things to ensure before you start running this
applicaton are

- Docker daemon is running ( In my case i am running docker for mac )
- Openshift is running ( I am using minishift but you could potentially use the real version )
- oc tool ( The client for accessing openshift ) is in the path and can be accessed from any terminal window


## Introduction

The application is a simple rest end point hosted by camel that uses netty as its http layer. It returns
the string "Hello world" when invoked.it's not a applicaton that is "deployed" on a server
in fact it comes bundled with an http layer, a la spring boot. The maven file for this project has three plugins
that 

- Create a runnable jar
- Create a docker image and push it up to docker hub
- Pull down the docker image from docker hub and run it on open shift, also create a route

## The bundler

The very first step in the process is to build a jar file that is "runnable", The application itself is nothing more
than a camel route, and a blocking camel main to keep it running. We are using the maven-assembly-plugin to
create a jar with all the dependencies bundled up

## Logging

The application uses the ubiquitous log4j logging framework and all the logging
is routed to a file /tmp/log.out that is configured in the log4j.properties file in 
src/main/resources folder of the project. 

## file beat

I have included the filebeat utility ( committed the sin of checking in the binary ) that watches the logfile (/tmp/log.out) created by the application
and establishes a pipeline to send the logs to the ELK stack. We are also leveraging the openshift feature
of config map which is a bunch of properties that are injected into a container and used in the application as
properties. In our case the elk url is in the config map. and can be changed on openshift ( console or UI)
to take effect on the pods. We also have to adjust the yml configuration file of file beat so that the
properties file is replaced with an environment variable substitution 
[ELK environment variable](https://www.elastic.co/guide/en/beats/filebeat/1.2/using-environ-vars.html). 


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

## The code

You can find the code here [Da project](https://github.com/svsvenu/poc/tree/master/camel-standalone-rest)



