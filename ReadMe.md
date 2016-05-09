Camel ActiveMQ HornetQ JMS Bridge Project
=============================

This project demonstrates a JMS bridge hosted on JBoss AMQ, by forwarding messages from ActiveMQ  to HornetQ:

	queueA (AMQ) -> queueB (HornetQ) -> queueC (AMQ)

After messages are forwarded to HornetQ, AMQ consumes messages from the same HornetQ queue and writes them to a new queue (queueC).  This is essentially a loopback test.

Apache Camel is used, allowing the bridge to be started / stopped via the HawtIO console

Method
-------------

(1) Install JBoss A-MQ (jboss-a-mq-6.2.0.redhat-133)

(2) Install JBoss EAP (jboss-eap-6.4.0)

(3) Create a test user on EAP to run the example

	<jboss eap dir>/bin/add-user.sh
	Username : quickstartUser
	Password : quickstartPwd1!

(4) Start EAP
	
	<jboss eap dir>/bin/standalone.sh -c standalone-full.xml
	
(5) Create the JMS queue on EAP

	<jboss eap dir>/bin/jboss-cli.sh
	connect
	jms-queue add --queue-address=queueB --entries=queue/queueB,java:jboss/exported/jms/queue/queueB


(6) Build this project via the command line

    mvn install

(7) Start A-MQ and deploy via the Karaf console

	<amq dir>/bin/amq
    osgi:install -s mvn:com.redhat/amq-hornetq-jms-bridge/1.0.0-SNAPSHOT
    
Testing
-------------

(1) Login to the [HawtIO](http://localhost:8181) console using Chrome / Firefox
(2) Navigate to the ActiveMQ Menu Tab
(3) Click on "queueA" and click the Send button
(4) Enter a test message then click "Send Message"
(5) Observe the message is forwarded to "queueC"
(6) Observe the logs by navigating to the "Logs" tab.  You'll notice the trace output of the message being transferred between A-MQ and Hornet-Q
(7) Try out starting / stopping each route via the "Camel" menu and experiment with flow control of messages

Help
-------------

For more help see the Red Hat support [article](https://access.redhat.com/solutions/739283) on creating an A-MQ / HornetQ JMS bridge:

    
