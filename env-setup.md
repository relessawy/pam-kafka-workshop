# Environment Setup

## Red Hat Process Automation Manager

In order to follow these guided labs, you should have RHPAM 7.9+ in your local environment.

If you need to setup PAM locally, you can use this repository to help you get up and running quickly: [Red Hat PAM Install Demo](https://github.com/jbossdemocentral/rhpam7-install-demo)

## Kafka locally

Red Hat supports the integration between RHPAM and AMQ Streams (Kafka). To follow the labs, you should have an accessible Kafka server. The KIE Server (process engine) will communicate with the topics that we will create in this server. 

If you don't have an environment available you can get a Kafka (Strimzi) server quickly running by using Docker. 

1. Clone this repository to your local machine.

	```
	$ git clone https://github.com/hguerrero/amq-examples
	```
	The docker-compose file available in this quickstart should bootstrap everything you need to have your Strimzi up and running: Zookeeper, Kafka server v2.5.0, Apicurio Registry and a Kafka Bridge. 

2. Access the `amq-examples/strimzi-all-in-one/` folder:
	```
	$ cd amq-examples/strimzi-all-in-one/
	```

3. Start the Kafka environment:
	```
	docker-compose up 
	```

Done. You now have a Kafka server running on localhost port 9092. 

## Next steps

Before proceeding to the next steps, make sure you have Red Hat PAM and Kafka server up and running.
