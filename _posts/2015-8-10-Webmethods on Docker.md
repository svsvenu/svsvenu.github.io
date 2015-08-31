

#Installing webmethods as a docker image


##**Assumptions**

	This blog assumes that you are familiar with docker and webmethods.This will only guide you through
	the steps of installing webmethods on a docker container and be able to run it as you would any other
	docker image.
	
##**Challenges**

	There are certain challenges that are unique to 'installable' software and docker. For example, if you want
	to install jboss as an image, you would just copy the installation directory and its ready to go. But
	with software that needs installation you would need to copy the installers into the base image and then
	install the software inside the container
	
##**Software**##

	You would need docker installed on your windows/mac/linux machine. You can chose the right one for your flavor of os below 
[docker-installation](https://docs.docker.com/installation).If you need a trial installation of webmethods, you can download it from below
[Webmethods-installation](http://techcommunity.softwareag.com/ecosystem/communities/public/webmethods/contents/download/)

##Steps to build the image##


###Step 1 - Pull the webmethods installation images

The webmethods installer downloads 2 files. An image file and an installer file. In addition to these two files you will also have
a zip file sent to your url that has the all the keys that the installer needs. Copy all the files into one folder

![_config.yml]({{ site.baseurl }}/images/saginstaller.png)


###Step 2 - Create a docker file 

Create a file called 'Dockerfile' (case sensitive ) in the same directory as the ones we put the installer and images in.The contents of the
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




	
	


