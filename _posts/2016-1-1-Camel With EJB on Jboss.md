---
layout: post
title: Camel With EJB
---

##Assumptions##

If you have installed a Fuse service works installation of EJB and chose not to use the switchyard
integration platform but use the camel that comes bundled with it for your integration application,
this blog will guide you through the set up process.

##Software##

Jboss FSW installation, Jboss developer studio or eclipse

##Bootstrapping the camel context

There are many ways of doing something on startup when it comes to a EE application. The easiest way (atleast to me)
is the singleton startup stateless session bean. Slap on a couple of annotations to your class and you are 
on your merry way. This can be seen in 

```

<dependency>
   <groupId>org.jboss.logging</groupId>
   <artifactId>jboss-logging</artifactId>
   <version>3.0.0.CR1</version>
   <scope>provided</scope>
</dependency>

```

##Configuring the context







