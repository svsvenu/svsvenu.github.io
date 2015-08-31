
---
Installing webmethods as a docker image
---

#**Assumptions**

	This blog assumes that you are familiar with docker and webmethods.This will only guide you through
	the steps of installing webmethods on a docker container and be able to run it as you would any other
	docker image.
	
#**Challenges**

	There are certain challenges that are unique to 'installable' software and docker. For example, if you want
	to install jboss as an image, you would just copy the installation directory and its ready to go. But
	with software that needs installation you would need to copy the installers into the base image and then
	install the software inside the container
	
###*Software*###

	You would need docker installed on your windows/mac/linux machine. You can chose the right one for your flavor of os
	at [docker-installation](dockerurl).
	
	
	
[dockerurl]:https://docs.docker.com/installation

