---
layout: post
title: Embedded Jboss mq broker with a network connector
---

##Assumptions

This blog assumes familiarity with JMS and Jboss AMQ as a provider. Please note that even though jboss MQ is based built on
active-mq ( the apache open source project ). It is packaged and shipped differently. This blog is based on the Jboss version
although it might be extended to the AMQ version 

##Software

You would be requiring a Jboss active mq installation, It can be found at [jboss-amq](http://www.jboss.org/products/amq/download/)

###Step 1 - Broker setup

![_config.yml]({{ site.baseurl }}/images/amq/amq.png)

###Creating embedded brokers

You can programmatically create brokers like below. I have chosen to have 3 brokers amq1, amq2, amq3

{% highlight java %}

			BrokerService broker = new BrokerService();

			broker.setBrokerName(brokerName);
		

{% endhighlight %}

###Adding connectors

Network ports can (should in most cases ) can be opened on each of these brokers. This can be done as follows. We will
open up 2 ports

{% highlight java %}
			
	        broker.addConnector("tcp://localhost:" + jmsPortNum);
	        
	        broker.addConnector("tcp://localhost:" + networkPortNum);

{% endhighlight %}

###Adding network connectors

Network connectors are used internally by the network of brokers to forward messages for load balancing. They
are configured on the ports that we opened on the broker. The brokers are connected in such a way that the
network connector on broker1 is set to connect to broker2 establishing a one-way communication from broker1 to
broker2. The same is done from broker2 to broker3. 

{% highlight java %}
			
	        broker.addConnector("tcp://localhost:" + jmsPortNum);
	        
	        broker.addConnector("tcp://localhost:" + networkPortNum);

{% endhighlight %}

###Producers and consumers

With the setup of the three brokers as above, we will now connect a producer to the queue on broker1 and a consumer on
broker3. The producer will send a message to queue called 'TEST' on broker1 and the consumer is looking for messages on 
a queue called 'TEST' on broker3. The message that was sent to broker1 will go to broker2 and on to broker3 making 2 hops
to reach the consumer on 3.



