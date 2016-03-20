---
layout: post
title: Venu personal notes
---

##Tomcat##
Go to tomcat directory

cd /Users/venusurampudi/Downloads/apache-tomcat-7.0.62

To start tomcat in jpda mode 

./catalina.sh jpda start


##MongoDB##

Go to Mongodb
cd /Users/venusurampudi/Downloads/mongodb-osx-x86_64-enterprise-3.2.3/bin

Jboss FSW installation, Jboss developer studio or eclipse

##Bootstrapping the camel context

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

##Fixing the deployment

Since we are using the cxf component, the deployment will complain about not being able to find the class
The exception you will see is below

```

Caused by: java.lang.ClassNotFoundException: org.apache.camel.component.cxf.common.header.CxfHeaderFilterStrategy from [Module "org.apache.camel.cxf:main" from local module loader @2f41b041 (finder: local module finder @19a93a4 (roots: /Users/venu/jboss-eap-6.1/modules,/Users/venu/jboss-eap-6.1/modules/system/layers/soa,/Users/venu/jboss-eap-6.1/modules/system/layers/sramp,/Users/venu/jboss-eap-6.1/modules/system/layers/base))]
	at org.jboss.modules.ModuleClassLoader.findClass(ModuleClassLoader.java:196) [jboss-modules.jar:1.2.2.Final-redhat-1]
	at org.jboss.modules.ConcurrentClassLoader.performLoadClassUnchecked(ConcurrentClassLoader.java:444) [jboss-modules.jar:1.2.2.Final-redhat-1]
	at org.jboss.modules.ConcurrentClassLoader.performLoadClassChecked(ConcurrentClassLoader.java:432) [jboss-modules.jar:1.2.2.Final-redhat-1]
	at org.jboss.modules.ConcurrentClassLoader.performLoadClass(ConcurrentClassLoader.java:374) [jboss-modules.jar:1.2.2.Final-redhat-1]


```

If we learnt the hard way that reading the exceptions is a helpful excercise, we need to add the following jar file
camel-cxf-transport-2.16.1 to the org.apache.camel.cxf module and update the module.xml file with the following
<resource-root path="camel-cxf-transport-2.16.1.jar"/> . You can download the the file from 
[Maven-repo](http://mvnrepository.com/artifact/org.apache.camel/camel-cxf-transport/2.16.1)

After this, it will start whining about modules not being available as dependencies. You would need to add the following
entries in the module.xml file to get it to resolve all of its class path dependencies

```
 <module name="javax.xml.ws.api"/>

 <module name="javax.jws.api"/>

 <module name="javax.wsdl4j.api"/>


```

This should resolve all the class path issues.

 









