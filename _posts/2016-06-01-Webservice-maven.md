---
layout: post
title: WSDL first webservice (provider)
---

##Introduction##

This blog explains the use of a maven plugin to import a wsdl that is provided. Usually WSDLs come
broken down as WSDL files and XSD files, the break down gives organizations a fake sense of professionalism
when its perfectly ok to stick em all in one file, yea i said it.

##Maven setup##

One of the reasons i am writing this blog is the options available for pulling in a WSDL, as one who easily overwhelmed by
choices i want to document one method that works all the way. The plugin i am using has the co-ordinates below

```
         <groupId>org.codehaus.mojo</groupId>
                    <artifactId>jaxws-maven-plugin</artifactId>
                    <version>1.12</version>
```










