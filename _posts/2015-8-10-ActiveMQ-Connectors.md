---
layout: post
title: Embedded broker with a network connector
---



##Assumptions##

Establish a connection from Embedded broker to a broker instance running outside a JVM in jboss MQ

Jboss AMQ provides a way of sending messages locally(Within the same JVM) to an embedded broker that created/accessed within the same JVM. 
This broker can in turn forward messages to a broker instance that is running on a separate instance
You can find the code to create and forward messages below

