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

##Creating an embedded broker##

![_config.yml]({{ site.baseurl }}/images/amq/amq.png)


You can programmatically create a broker in java in a stand alone client like below

{% highlight java %}
package com.venu.activemq;

import java.net.URI;

import javax.jms.Connection;
import javax.jms.DeliveryMode;
import javax.jms.Destination;
import javax.jms.Message;
import javax.jms.MessageConsumer;
import javax.jms.MessageProducer;
import javax.jms.Session;
import javax.jms.TextMessage;

import org.apache.activemq.ActiveMQConnectionFactory;
import org.apache.activemq.broker.BrokerService;
import org.apache.activemq.network.NetworkConnector;


public class EmbeddedBrokerToRemoteBroker {
	
	public static void main(String[] args)
	{
		try{
		BrokerService broker = new BrokerService();
		// configure the broker
		broker.setBrokerName("localhost");
	//	NetworkConnector connector = broker.addNetworkConnector("static://"+"tcp://localhost:61618");
	//	connector.setDuplex(true);

	//	broker.addConnector("tcp://localhost:61618");
		broker.setUseJmx(false);
		
		
		NetworkConnector connectorRmote = broker.addNetworkConnector("static:(tcp://localhost:61616)?maxReconnectDelay=5000&useExponentialBackOff=false");
		
		connectorRmote.setUserName("admin");
		
		connectorRmote.setPassword("admin");

		
	//	broker.addNetworkConnector(new URI("static:(tcp://localhost:61616)?maxReconnectDelay=5000&useExponentialBackOff=false&Username=admin"));
		
		broker.start();
		
		System.out.println( broker.getNetworkConnectorURIs() );
		
		ActiveMQConnectionFactory cf = new ActiveMQConnectionFactory("vm://localhost?broker.persistent=false");
		
//
		Connection connection = cf.createConnection(); 
        connection.start(); 
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        Destination destination = session.createQueue("TEST.FOO2");
        MessageProducer producer = session.createProducer(destination);
        producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);
        String text = "Hello world! From: embedded broker in its own jvm " + Thread.currentThread().getName() + " : " ;
        TextMessage message = session.createTextMessage(text);
        System.out.println("Sent message: "+ message.hashCode() + " : " + Thread.currentThread().getName());
        
       
        int i=1;
        while (i<10) { 
        producer.send(message);
        Thread.sleep(1000);
        i++;
        } 
        session.close();
        connection.close();
        
        broker.stop();

		
		}
		
		catch(Exception e){
			
			e.printStackTrace();
		}
		
		
	}
}

}
{% endhighlight %}

Establish a connection from Embedded broker to a broker instance running outside a JVM in jboss MQ

Jboss AMQ provides a way of sending messages locally(Within the same JVM) to an embedded broker that created/accessed within the same JVM. 
This broker can in turn forward messages to a broker instance that is running on a separate instance
You can find the code to create and forward messages below

