---
layout: post
title: Venu personal notes
---

## Tomcat
Go to tomcat directory

cd /Users/venusurampudi/Downloads/apache-tomcat-7.0.62

To start tomcat in jpda mode 

./catalina.sh jpda start

## MongoDB

Go to Mongodb
cd /Users/venusurampudi/Downloads/mongodb-osx-x86_64-enterprise-3.2.3/bin

Start mongodb
 ./mongod --dbpath  data/db

Jboss FSW installation, Jboss developer studio or eclipse

## Bootstrapping the camel context

There are many ways of doing something on startup when it comes to a EE application. The easiest way (atleast to me)
is the singleton startup stateless session bean. Slap on a couple of annotations to your class and you are 
on your merry way. The application if using cxf will however will not deploy as the installation has some missing
jar files. The deployment will flame out complaining about header filter

```

<dependency>
   <groupId>org.jboss.logging</groupId>
   <artifactId>jboss-logging</artifactId>
   <version>3.0.0.CR1</version>
   <scope>provided</scope>
</dependency>

```







