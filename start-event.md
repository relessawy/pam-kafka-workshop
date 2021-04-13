# The Credit Card Raise Approval Project

In this use case we should handle the automation of a credit card limit raise approval process. Most card issuers allow customers to request an increased credit limit through their websites, mobile apps or over the phone. Let’s consider we need to deliver this automation for a bank that wants to achieve a similar use case within an event-driven architecture.

The first task you have it to enable an existing process to react to events that are published in a specific topic. Whenever a new event is published, a new process instance should be created.

## Importing the project

Let's import the existing project so we can start implementing the eventing capabilities. 

1. Access business central, and import the following project: https://github.com/kmacedovarela/cc-limit-approval-app-step1
  ![]({% image_path bc-import-project.png %}){:width="600px"}

2. Let's check the existing project. Open the `cc-limit-raise-approval` process. Notice it is a traditional process. Processes like this can be started either via REST or JMS.
  ![]({% image_path bc-start-process.png %}){:width="600px"}

## Updating the process definition

3. To allow this process definition to be started with events, the first step is to change the start event to a *start message event*:
  ![]({% image_path bc-convert-start-event.png %}){:width="400px"}

4. Whenever a customer make a new request (independently of the channel used) an event should be published on the `new-requests` Kafka `topic`. With that, a new process instance should be started whenever a new event is published in this topic. Let's configure the *start message event*: 
  ![]({% image_path bc-start-message-config.png %}){:width="400px"}

  **Important:** we need to receive the data that is the event data. The KIE Server provides automatic marshalling to help us mapping the input directly to a _Data Object_ (a POJO). This project has an object named `LimitRaiseRequest.java` which we will use to receive the incoming data. 

5. On the properties panel of the _Start Message Event_, configure the input data:

  * Name: request
  * Data Type: `LimitRaiseRequest`
  * Target: request

  ![]({% image_path bc-start-msg-parameters.png %}){:width="400px"}

6. Save the process. Your process should now look like this:
  ![]({% image_path bc-process-step1.png %}){:width="400px"}

## Deploying the project

Now, let's deploy and test the project. 

1. On the breadcrumb, click on "cc-limit-approval-app-step1" to go back to the Project Explorer view.

2. Click on the "Deploy" button.

## Testing the project

1. Let's publish a new event in the `incoming-requests` topic using the Kafka producer CLI tool. Open a new tab in your terminal and access the `strimzi-all-in-one` project folder. 

  ~~~
  $ cd $PROJECTS_DIR/amq-examples/strimzi-all-in-one
  $ docker-compose exec kafka bin/kafka-console-producer.sh --topic incoming-requests --bootstrap-server localhost:9092
  ~~~

2. After that, Kafka producer will listen to new messages. You can send the following data, and press enter:

  ~~~
  {"data" : {"customerId": 1, "customerScore": 250, "requestedValue":1500}}
  ~~~

3. Back to the browser, open Business Central. On the top menu, go to **Menu -> Manage -> Process Instances**. 

4. On the left column, filter by "Completed" State. You should see as many instances as the number of events you published on Kafka:

  ![]({% image_path bc-lab-one-process-instances.png %}){:width="600px"}

5. Select a process instance, and next, select the tab `Diagram`. You should see something like: 

  ![]({% image_path bc-lab-one-completed-process-instance-diagram.png %}){:width="600px"}


