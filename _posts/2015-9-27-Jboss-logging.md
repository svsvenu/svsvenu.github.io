---
layout: post
title: Jboss application logging
---

##Assumptions##

Need to know a little bit about log4j.This shows some of the cool features of jboss logging

##Software##

Jboss EAP6

##Setting up an application to use Jboss logging

If you choose to use jboss logging as the logging back bone for the application you are building
then you would need to import the jars into your projects. The maven co-ordinates for this are

```

<dependency>
   <groupId>org.jboss.logging</groupId>
   <artifactId>jboss-logging</artifactId>
   <version>3.0.0.CR1</version>
   <scope>provided</scope>
</dependency>

```

If you still are using ant, please dont make fun of your mom saying she cant work the smart phone. The 
scope is set to provided as the Jboss EAP server comes built in with jboss logging which is essentially
a wrapper over log4j.

##Importing the classes

To use jboss logging, import the org.jboss.logging.Logger. to instantiate it use 

{% highlight java %}

	private static final Logger logger = Logger.getLogger(WelcomeBean.class.getName());
	
	{% endhighlight %}
	
The rest of the api is just like log4j.

{% highlight java %}
       
       logger.debug("DEBUG");
       logger.info("INFO");
       logger.warn("WARN");
       logger.error("ERROR");
       logger.fatal("FATAL");

	{% endhighlight %}

##Logger hierarchy


