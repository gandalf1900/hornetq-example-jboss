
#### Introduction
--------------------
This hornetQ demo starts up a configured embebbed hornetQ server running inside Wildfly 8.1.0.Final.
It also demnostrate how a client, the JMSClient java client, communicates with this hornet queue.

#### Add an Application User
----------------
This demo uses secured management interfaces and requires that you create an application user to access the running application.
Run from BOSS_HOME\bin add-user.bat -a -u myuser -p mypass123456 -g guest. 
This creates a application user myuser with a correct role.

Reference: https://github.com/jboss-developer/jboss-developer-shared-resources

#### Configure JMS 
-------------------------------------

1. Copy over the config/standalone-full.xml to your local Jboss server. This adds a jsm queue called testQueue to the Wildfly messaging subsystem.
Copy it to your JBOSS_HOME/standalone/configuration/standalone.xml.
The config/standalone.xml includes all needed JMS setup to havee hornetQ up and running when jboss server is started.

2. Start the server with -c standalone-full.xml option. 
For example JBOSS_HOME\bin\standalone.bat -c standalone-full.xml

Watch out for hornetQ starting up:
                                                
....
15:57:11,679 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221000: live server is starting with configuration HornetQ Configuration (clustered=false,backup=false,sharedStore=true,journalDirectory=C:\apps\wildfly-8.1.0.Final\standalone\data\messagingjournal,bindingsDirectory=C:\apps\wildfly-8.1.0.Final\standalone\data\messagingbindings,largeMessagesDirectory=C:\apps\wildfly-8.1.0.Final\standalone\data\messaginglargemessages,pagingDirectory=C:\apps\wildfly-8.1.0.Final\standalone\data\messagingpaging)
15:57:11,680 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221006: Waiting to obtain live lock
15:57:11,700 INFO  [org.jboss.as.jacorb] (MSC service thread 1-3) JBAS016328: CORBA Naming Service started
15:57:11,710 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221013: Using NIO Journal
15:57:11,731 INFO  [io.netty.util.internal.PlatformDependent] (ServerService Thread Pool -- 56) Your platform does not provide complete low-level API for accessing direct buffers reliably. Unless explicitly requested, heap buffer will always be preferred to avoid potential system unstability.
15:57:11,750 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221043: Adding protocol support CORE
15:57:11,757 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221043: Adding protocol support AMQP
15:57:11,760 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221043: Adding protocol support STOMP
15:57:11,785 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221034: Waiting to obtain live lock
15:57:11,785 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221035: Live Server Obtained live lock
15:57:11,816 INFO  [org.jboss.ws.common.management] (MSC service thread 1-8) JBWS022052: Starting JBoss Web Services - Stack CXF Server 4.2.4.Final
15:57:11,883 INFO  [org.jboss.messaging] (MSC service thread 1-7) JBAS011615: Registered HTTP upgrade for hornetq-remoting protocol handled by http-acceptor-throughput acceptor
15:57:11,883 INFO  [org.jboss.messaging] (MSC service thread 1-2) JBAS011615: Registered HTTP upgrade for hornetq-remoting protocol handled by http-acceptor acceptor
15:57:11,949 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221007: Server is now live
15:57:11,949 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221001: HornetQ Server version 2.4.1.Final (Fast Hornet, 124) [fd400928-22d8-11e4-870c-d1a35fd87618] 
15:57:11,952 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 56) HQ221003: trying to deploy queue jms.queue.ExpiryQueue
15:57:11,956 INFO  [org.jboss.as.messaging] (ServerService Thread Pool -- 56) JBAS011601: Bound messaging object to jndi name java:/jms/queue/ExpiryQueue
15:57:11,957 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 60) HQ221003: trying to deploy queue jms.queue.DLQ
15:57:11,957 INFO  [org.jboss.as.messaging] (ServerService Thread Pool -- 60) JBAS011601: Bound messaging object to jndi name java:/jms/queue/DLQ
15:57:11,969 INFO  [org.jboss.as.messaging] (ServerService Thread Pool -- 58) JBAS011601: Bound messaging object to jndi name java:/ConnectionFactory
15:57:11,970 INFO  [org.hornetq.core.server] (ServerService Thread Pool -- 59) HQ221003: trying to deploy queue jms.queue.testQueue
15:57:11,971 INFO  [org.jboss.as.messaging] (ServerService Thread Pool -- 59) JBAS011601: Bound messaging object to jndi name queue/test
15:57:11,971 INFO  [org.jboss.as.messaging] (ServerService Thread Pool -- 59) JBAS011601: Bound messaging object to jndi name java:jboss/exported/jms/queue/test
15:57:11,971 INFO  [org.jboss.as.messaging] (ServerService Thread Pool -- 57) JBAS011601: Bound messaging object to jndi name java:jboss/exported/jms/RemoteConnectionFactory
15:57:12,006 INFO  [org.jboss.as.connector.deployment] (MSC service thread 1-5) JBAS010406: Registered connection factory java:/JmsXA
15:57:12,033 INFO  [org.hornetq.ra] (MSC service thread 1-5) HornetQ resource adaptor started
....


#### Start the JMSClient 
-------------------------------------
Start the JMSClient.

You should see something like this:

aug 14, 2014 8:44:56 AM org.xnio.Xnio <clinit>
INFO: XNIO version 3.2.2.Final
aug 14, 2014 8:44:56 AM org.xnio.nio.NioXnio <clinit>
INFO: XNIO NIO Implementation Version 3.2.2.Final
aug 14, 2014 8:44:56 AM org.jboss.remoting3.EndpointImpl <clinit>
INFO: JBoss Remoting version 4.0.3.Final
aug 14, 2014 8:44:56 AM no.web.client.JMSClient main
INFO: Attempting to acquire connection factory "jms/RemoteConnectionFactory"
aug 14, 2014 8:44:57 AM no.web.client.JMSClient main
INFO: Found connection factory "jms/RemoteConnectionFactory" in JNDI
aug 14, 2014 8:44:57 AM no.web.client.JMSClient main
INFO: Attempting to acquire destination "jms/queue/test"
aug 14, 2014 8:44:57 AM no.web.client.JMSClient main
INFO: Found destination "jms/queue/test" in JNDI
aug 14, 2014 8:44:57 AM no.web.client.JMSClient main
INFO: Sending 1 messages with content: Thou shall not pass! 
aug 14, 2014 8:44:57 AM no.web.client.JMSClient main
INFO: Received message with content Thou shall not pass! 