---
layout: post
title: WSDL first webservice
---

## Introduction

This blog explains the use of a maven plugin to import a wsdl that is provided. Usually WSDLs come
broken down as WSDL files and XSD files, the break down gives organizations a fake sense of professionalism
when its perfectly ok to stick em all in one file, yea i said it.

## Maven setup

One of the reasons i am writing this blog is the options available for pulling in a WSDL, as one who easily overwhelmed by
choices i want to document one method that works all the way. The plugin i am using has the co-ordinates below

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













