# The Credit Card Raise Approval Project

In this use case we should handle the automation of a credit card limit raise approval process. Most card issuers allow customers to request an increased credit limit through their websites, mobile apps or over the phone. Let’s consider we need to deliver this automation for a bank that wants to achieve a similar use case within an event-driven architecture.

The first task you have it to enable an existing process to react to events that are published in a specific topic. Whenever a new event is published, a new process instance should be created.

## Importing the project

Let's import the existing project so we can start implementing the eventing capabilities. 

1. Access business central, and import the following project: https://github.com/kmacedovarela/cc-limit-approval-app-step1
  ![]({% image_path bc-import-project.png %}){:width="600px"}

2. Let's check the existing project. Open the `cc-limit-raise-approval` process. Notice it is a traditional process. Processes like this can be started either via REST or JMS.
  ![]({% image_path bc-start-process.png %}){:width="600px"}

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





