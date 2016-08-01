---
layout: post
title: Peer to Peer  Jboss data grid
---

## Introduction

This blogs explains the setup of a distributed cache whose keys and values are dispersed across nodes that are
set up to be part of the the cluster. The access to the keys and values from any of the participating nodes is
as though its available locally, there is no need for a centralized server ( although it is supported ) to serve up
they keys, nodes can join and leave the cluster as they choose to. The grid will attempt to rebalance the load across all
the available nodes ( evenly?). 1k word version

![_config.yml]({{ site.baseurl }}/images/jdg/jdg.png)




## I have a route, can some one please Eff'n deploy the Eff'n route in an Eff'n containter

You Eff's have been answered. Say hello to the brand spanking new Jboss fuse 6.2.1. It comes in two flavors, OSGI and EJB. I have
adopted the EJB version courtesy familiarity and market demand.


```
         <groupId>org.codehaus.mojo</groupId>
         <artifactId>jaxws-maven-plugin</artifactId>
         <version>1.12</version>
```
If my blog stands the test of time, the version might change and this would be 'classic'. The plugin then needs to be told
about a few obvious things, where's the dang WSDL, what is the root package name, whose door step do i drop new born generated files.
To weed the mundane from my otherwise mundane life, i am also using a plugin that deploys the war file to the locally sourced
Jboss server. Turnkey ya'll

## Plugin exploits

After we have run a clean install on the POM file, and hit refresh on our project we should be able to see the spoils of our plugin
run. a folder called 'generated' is created and the root folder that was supplied to the plugin configuration is created, the generated
files (JaxB annotated ) are created underneath it.Two classes of particular interest are the interface that represents the port and the
class that represents the Service.

## The programmer's work

Ok sir/mam, a lot of work has been done for you by the benevolent open source community, but you want to leave a mark (stain?) too. We would
begin by implementing the port interface, in our case its "HumanResource". In addition to implementing it, we have to let the EE container
know that your intentions are to expose it as a webservice, hellooo annotations. The class itself is annotated as webservice(javax.jws.WebService;) and the the method
is annotated as Webmethod(javax.jws.WebMethod). Now that you are handed the java objects, you can turn around and do your business, no pun intended.

## Fruits of labor

Enjoy them while they still ripe. The war should deploy fine and tell you that your webservice is now available for consumption. A quick cyberhunt on
google will tell you that SOAPUI is a popular tool to test your webservice and it's got to to true if its on the internet.













