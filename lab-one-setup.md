# Labs - Setup

In order to be able to start processes based on new events, we will need to set up our environment.

In this setup we will:
- Create the Kafka topics we will use on the guided labs
- Configure Red Hat PAM and enable the Kafka capabilities

## Creating the Kafka topics

Considering you are using the Kafka we set up on the previous labs, you can create the new topic by:

1. Open a new terminal and access the project folder:

```
$ cd amq-examples/strimzi-all-in-one/
```

2. Create the three topics:

~~~
docker-compose exec kafka bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic incoming-requests

docker-compose exec kafka bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic requests-approved

docker-compose exec kafka bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic requests-denied
~~~

## Configuring Red Hat PAM

Now, we should configure Red Hat PAM Kafka extension. This is the extension that allows the KIE Server (process and decision engine) to react to events and publish events to kafka topics.

**INFO:** There are several options in PAM to customize the Kafka address, topic names, etc. In our case, we’re using the default Kafka address, which is, localhost:9092. More customization information can be found in the official Red Hat product documentation: Configuring a KIE Server to send and receive Kafka messages from the process.

In this setup steps, we will configure PAM only in the server level - _we are not yet configuring the business project_. We will configure the project as we move forward on the labs.

*Enabling the Kafka extension*

We can configure the engine to support different capabilities. In order to enable processes to be started through eventing, we only need to enable the extension via system property. 

1. With PAM up and running, execute: 

~~~
$JBOSS_HOME/bin/jboss-cli.sh -c
[standalone@localhost:9990 /] /system-property=org.kie.kafka.server.ext.disabled:add(value=false)
[standalone@localhost:9990 /] :shutdown(restart=true)
~~~

The first command will enable the Kafka extension. Next, we're reestarting EAP so that the new configuration is active. You can check EAP logs to confirm it is restarting up.

The following log shows up in PAM logs: 

~~~
INFO  [org.kie.server.services.impl.KieServerImpl] (ServerService Thread Pool -- 74) Kafka KIE Server extension has been successfully registered as server extension
~~~

This is the only configuration you will need in a server level to be able to start processes using events.