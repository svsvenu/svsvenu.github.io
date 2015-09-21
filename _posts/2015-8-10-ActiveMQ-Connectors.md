---
layout: post
title: Embedded Jboss mq broker with a network connector
---

##Assumptions##

This blog assumes familiarity with JMS and Jboss AMQ as a provider. Please note that even though jboss MQ is based built on
active-mq ( the apache open source project ). It is packaged and shipped differently. This blog is based on the Jboss version
although it might be extended to the AMQ version 

##Software##

You would be requiring a Jboss active mq installation, It can be found at [jboss-amq](http://www.jboss.org/products/amq/download/)

###Step 1 - Broker setup

![_config.yml]({{ site.baseurl }}/images/amq/amq.png)

###Creating embedded brokers###

You can programmatically create brokers like below. I have chosen to have 3 brokers amq1, amq2 and amq3

{% highlight java %}

			BrokerService broker = new BrokerService();

			broker.setBrokerName(brokerName);
		

{% endhighlight %}

###Adding connectors###

Network ports can (should in most cases ) can be opened on each of these brokers. This can be done as follows. We will
open up 2 ports

{% highlight java %}
			
	        broker.addConnector("tcp://localhost:" + jmsPortNum);
	        
	        broker.addConnector("tcp://localhost:" + networkPortNum);

{% endhighlight %}

###Adding network connectors###

Network connectors are used internally by the network of brokers to forward messages for load balancing. They
are configured on the ports that we opened on the broker


Establish a connection from Embedded broker to a broker instance running outside a JVM in jboss MQ

Jboss AMQ provides a way of sending messages locally(Within the same JVM) to an embedded broker that created/accessed within the same JVM. 
This broker can in turn forward messages to a broker instance that is running on a separate instance
You can find the code to create and forward messages below

