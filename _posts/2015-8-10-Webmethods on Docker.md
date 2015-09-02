---
layout: post
title: Installing webmethods as a docker image
---



##Assumptions

	This blog assumes that you are familiar with docker and webmethods.This will only guide you through
	the steps of installing webmethods on a docker container and be able to run it as you would any other
	docker image.
	
##Challenges

	There are certain challenges that are unique to 'installable' software and docker. For example, if you want
	to install jboss as an image, you would just copy the installation directory and its ready to go. But
	with software that needs installation you would need to copy the installers into the base image and then
	install the software inside the container
	
##Software##

	You would need docker installed on your windows/mac/linux machine. You can chose the right one for your flavor of os below 
[docker-installation](https://docs.docker.com/installation).If you need a trial installation of webmethods, you can download it from below
[Webmethods-installation](http://techcommunity.softwareag.com/ecosystem/communities/public/webmethods/contents/download/)

##Steps to build the image##


###Step 1 - Pull the webmethods installation images

The webmethods installer downloads 2 files. An image file and an installer file. In addition to these two files you will also have
a zip file sent to your url that has the all the keys that the installer needs. Copy all the files into one folder

![_config.yml]({{ site.baseurl }}/images/saginstaller.png)


###Step 2 - Create a docker file 

Create a file called 'Dockerfile' (case sensitive ) in the same directory as the ones we put the installer and images from step-1 in.The contents of the
docker file are below

```
### Set the base image to Fedora
FROM jboss/base-jdk:7

### File Author / Maintainer
MAINTAINER "Venu " "svsvenu@gmail.com"

## Switch the user to root
USER root

### Install Webmethods
ADD SoftwareAGInstaller20150415.jar /tmp/SoftwareAGInstaller20150415.jar
ADD webMFREEDownload98(Linux64bit).zip /tmp/webMFREEDownload98(Linux64bit).zip
ADD wM_v9.8_Free+Trial_Keys.zip /tmp/wM_v9.8_Free+Trial_Keys.zip

### Open Ports, for this simple example we are only going to open up the admin port
EXPOSE 5555


```

###Step 3 - Build the base docker image.

At this point, we need to save the image. We will be running the installer inside the docker image/container ( Still using the terms interchangeable,
there might be a day where i could differentiate the nuances ). Go to your docker terminal, cd to the directory where you put the installers
and Dockerfile and run the following command

```

docker build -q --rm -t svsvenu/wmbase .

```

The options explained below
	-q->
	
###Step 4 - Start the docker image

Start the docker image that you just build by running the following command. You would need the tail command at the end or else docker will
shut down the image as the main process has terminated. Appending the tail command keeps the process running and hence the container up

docker run -d -P svsvenu/wmbase tail -f /dev/null

If Everything went well you should see the container running, we could validate it by running the following

docker ps

###Step 5 - SSH into the docker container

We will now get into the container and finish our installation, i.e run the installer inside the container. To get into the container
run the following command

docker exec -i -t <container_id> bash

###Step 6 - Start the webmethods installation

Since we are inside the container without a GUI to help us, the webmethods installation has to be command line. This can be accomplished 
by running the following installer command.

cd /tmp

unzip wM_v9.8_Free+Trial_Keys.zip

java -jar SoftwareAGInstaller20150415.jar -console -readImage webMFREEDownload98\(Linux64bit\).zip -installDir /opt/webm

This should now begin a series of installation questions that you would need to answer with the help of your god given intelligence

After the installation is complete, be sure to delete the installer files as they are nolonger needed. This will save some valuable 

container size. 

There has to be a modification made to /opt/webm/IntegrationServer/bin/server.sh. Append the following line to it so that the integration
server runs in the container

while true; do sleep 1000; done

###Step 6 - Save the state of the image

Get out of the shell by typing CTRL+p+q

Commit the image after installation by running the following command

docker commit <container_id> svsvenu/wmbase


###Step 7 - Build a container that starts the Integration server

In a new directory Create the following docker file with name Dockerfile

```

### Set the base image to the one we just built
FROM svsvenu/wmbase

USER root

### File Author / Maintainer
MAINTAINER "Venu" "svsvenu@gmail.com"

EXPOSE 5555 
 
### start the IS 
ENTRYPOINT /opt/webm/IntegrationServer/bin/server.sh  

```

Rebuild this container as  

{% highlight java linenos%}

Public static void main (String args){
int i =0;

i++;
}


{% endhighlight  %}















	
	


