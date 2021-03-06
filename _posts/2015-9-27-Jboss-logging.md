---
layout: post
title: Jboss application logging
---

## Assumptions

Need to know a little bit about log4j.This shows some of the cool features of jboss logging

## Software

Jboss EAP6

## Setting up an application to use Jboss logging

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

## Importing the classes

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

## Logger hierarchy

This might be a known fact for most of you familiar with log4j, but this is something that never seems
to find a home is my permanent memory recesses. If iam in a class named

xxx.yyy.zzz.Myclass

and declare a logger like below

	private static final Logger logger = Logger.getLogger(xxx.yyy.zzz.Myclass.class.getName());

Inherently you established a hierarchy. The parent of xxx.yyy.zzz.Myclass is xxx.yyy.zzz, its parent
is xxx.yyy and so on. Hope you dont need help is getting the pattern. Levels are optionally set at the
logger level, if not, the parent's level is inherited.

## The cool part

If you have been scrolling your mouse wheels off to get the cool part, here is it. You can control
the log levels in run-time w/o requiring server restart or application reload or something crazy
your bored developer has written. 

## straight UI homey

To change the logger at run time, open the Jboss console and go to profile->core->logging. Find the picture
below in addition to my 1000 words

![_config.yml]({{ site.baseurl }}/images/logging/logging.png)

## Creating a custom handler

To add a handler that your logger uses, click on the handler tab, to add a custom handler. The Narcissist
that i am,  i named it venu-console

![_config.yml]({{ site.baseurl }}/images/logging/handler.png)

## Bring it home

To make many a coach happy, we are now going to tie the handler to the logger and put it all in a pretty
package that is sports center worthy.A sample code that uses jboss logging is below. Focus on the logging
part but feel free to snoop around

[This guy really wrote some code](https://github.com/svsvenu/blog/blob/master/kitchensink/src/main/java/com/venu/jsf/WelcomeBean.java)





