# Event-driven business processes

Event-driven processes can react to the events that happens in the ecosystem. Kafka is an open-source even streaming platform, and currently, one of the most popular tools. In this type of architecture, we have the Kafka `topics` used as the communication layer in between the services. Each service can now be considered a `consumer`or a `producer`, in other words, each service can publish or consume events to/from the `topics`.  

When using business applications within event driven architectures, you can think of different scenarios:

* New business process instances can be started based on events that are published in specific topics;
* Business processes can have intermediate events that can be activated based on events;
* Business processes can emit events during the process execution or at the end, allowing other services to react to the output of the execution; 
* Track every transaction committed either for business processes, cases (case management), or human tasks by publishing events.

## The alignment of tech evolution and business standards like BMPN

When providing an implementation for a specification, each provider has the opportunity to deliver the solution of choice. It is not different for the BPMN specification. It allows different implementations for its diagram elements, and this is how RHPAM delivers the most recent tech concepts by still allowing business users to use the modeling notation they are used to.

In RHPAM, it is possible to make use of message events (starting, intermediate or ending) to interact via events. In this case, the KIE Server Kafka extension makes sure the communication occurs effectively with the event streaming brokers.

## Next Steps

On the next step, we will create our first event-driven process. 