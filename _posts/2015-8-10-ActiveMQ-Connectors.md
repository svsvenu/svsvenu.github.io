---
layout: post
title: Embedded Jboss mq broker with a network connector
---

##Assumptions##

This blog assumes familiarity with JMS and Jboss AMQ as a provider. Please note that even though jboss MQ is based built on
active-mq ( the apache open source project ). It is packaged and shipped differently. This blog is based on the Jboss version
although it might be extended to the AMQ version 

##Software##




Establish a connection from Embedded broker to a broker instance running outside a JVM in jboss MQ

Jboss AMQ provides a way of sending messages locally(Within the same JVM) to an embedded broker that created/accessed within the same JVM. 
This broker can in turn forward messages to a broker instance that is running on a separate instance
You can find the code to create and forward messages below

##Software##

You would need docker installed on your windows/mac/linux machine. You can chose the right one for your flavor of os at 
[docker-installation](https://docs.docker.com/installation).If you need a trial installation of webmethods, you can download it from 
[Webmethods-installation](http://techcommunity.softwareag.com/ecosystem/communities/public/webmethods/contents/download/)
