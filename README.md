# Ballerina Azure Service Bus Connector

[![Build](https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/workflows/CI/badge.svg)](https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/actions?query=workflow%3ACI)
[![GitHub Last Commit](https://img.shields.io/github/last-commit/ballerina-platform/module-ballerinax-azure-service-bus.svg)](https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/commits/master)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Connects to Microsoft Azure Service Bus using Ballerina.

# Introduction

## What is Azure Service Bus?

The [Azure Service Bus](https://azure.microsoft.com/en-us/services/service-bus/) is a reliable cloud messaging as 
a service (Maas) and simple hybrid integration. It is an enterprise message broker with message queues and topics with 
publisher/subscriber capabilities. It is used to decouple applications and services from each other. Data is transferred 
between different applications and services using messages. You can find more information [here](https://docs.microsoft.com/en-us/azure/service-bus-messaging/).

The primary wire protocol for Service Bus is [Advanced Messaging Queueing Protocol](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-amqp-overview) 
(AMQP) 1.0, an open ISO/IEC standard and it can be used from any AMQP 1.0 compliant protocol client. Service Bus 
supports standard [AMQP 1.0]((https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-amqp-overview)) 
and [HTTP or REST](https://docs.microsoft.com/en-us/rest/api/servicebus/) protocols and their respective security 
facilities, including transport-level security (TLS). Clients can be authorized for access using [Shared Access Signature](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-sas) 
or [Azure Active Directory](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-authentication-and-authorization) 
role-based security.

### Azure Service Bus Concepts

1. Namespaces.
   A namespace is a container for all messaging components. We can have multiple queues and topics in a namespace. 
   The namespaces serve as application containers. In terms of terminology of other brokers, a namespace can be compared 
   to a "server", but the concepts aren't the same.
   
2. Queues.
   Messages are sent to and received from queues. The messages are stored in queues until the receiving application is 
   available to receive and process them. Messages are ordered and time stamped on arrival in queues. Messages are 
   delivered in pull mode where messages are delivered only when requested.
   ![image](docs/images/Azure_Service_Bus_Queue.png)

3. Topics.
   Topics are used to send and receive messages in publisher/subscriber models. Topics can have multiple, independent 
   subscriptions that are like queues, which attach to the topic from the receiver side. While a queue is often used 
   for point-to-point communication, topics are used in publish/subscribe scenarios. A subscriber to a topic can receive 
   a copy of each message sent to that topic. You can define rules on a subscription. A subscription rule has a filter 
   to define a condition for the message to be copied into the subscription and an optional action that can modify 
   message metadata.
   ![image](docs/images/Azure_Service_Bus_Topic_Subscription.png)

## Key Features of Azure Service Bus
* Load-balancing work across competing workers
* Safely routing and transferring data and control across service and application boundaries
* Coordinating transactional work that requires a high-degree of reliability

## Connector Overview
The Azure Service Bus Ballerina Connector is used to connect to the Azure Service Bus to send and receive messages. 
You can perform actions such as send to queue, send to topic, receive from queue, receive from subscription, etc.

![image](docs/images/Azure_Service_Bus_Ballerina_Connector.png)

[Azure Service Bus' primary protocol is AMQP 1.0](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-amqp-overview) 
and it can be used from any AMQP 1.0 compliant protocol client. 
This connector makes use of an AMQP 1.0 compliant protocol client for interoperability.

Service Bus provides a Microsoft supported native API and this connector makes use of this public API. 
Service Bus service APIs access the Service Bus service directly, and perform various management operations at the 
entity level, rather than at the namespace level (such as sending a message to a queue). This connector supports these 
basic operations. These APIs use Shared Access Signature(SAS) authentication and this connector supports SAS authentication.

# Prerequisites

* An Azure account and subscription.
  If you don't have an Azure subscription, [sign up for a free Azure account](https://azure.microsoft.com/free/).

* A Service Bus namespace.
  If you don't have [a service bus namespace](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-create-namespace-portal),
  learn how to create your Service Bus namespace.

* A messaging entity, such as a queue, topic or subscription.
  If you don't have these items, learn how to
    * [Create a queue in the Azure portal](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#create-a-queue-in-the-azure-portal)
    * [Create a Topic using the Azure portal](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal#create-a-topic-using-the-azure-portal)
    * [Create Subscriptions to the Topic](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal#create-subscriptions-to-the-topic)

* Java 11 Installed
  Java Development Kit (JDK) with version 11 is required.

* Ballerina SLP8 Installed
  Ballerina Swan Lake Preview Version 8 is required.

* Shared Access Signature (SAS) Authentication Credentials
    * Connection String
    * Entity Path

## Configuration
Instantiate the connector by giving authorization credentials that a client application can use to send/receive messages
to/from the queue/topic/subscription.

### Getting the authorization credentials

#### For Service Bus Queues

1. Make sure you have an Azure subscription. If you don't have an Azure subscription, you can create a
   [free account](https://azure.microsoft.com/en-us/free/) before you begin.

2. [Create a namespace in the Azure portal](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#create-a-namespace-in-the-azure-portal)

3. [Get the connection string](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#get-the-connection-string)

4. [Create a queue in the Azure portal & get Entity Path](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#create-a-queue-in-the-azure-portal). 
   It is in the format ‘queueName’.

#### For Service Bus Topics and Subscriptions

1. Make sure you have an Azure subscription. If you don't have an Azure subscription, you can create a
   [free account](https://azure.microsoft.com/en-us/free/) before you begin.

2. [Create a namespace in the Azure portal](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#create-a-namespace-in-the-azure-portal)

3. [Get the connection string](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#get-the-connection-string)

4. [Create a topic in the Azure portal & get Entity Path](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal#create-a-topic-using-the-azure-portal). 
   It's in the format ‘topicName‘.

5. [Create a subscription in the Azure portal & get Entity Path](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal#create-subscriptions-to-the-topic). 
   It’s in the format ‘topicName/subscriptions/subscriptionName’.
   

# Supported Versions & Limitations

## Supported Versions
|                     |    Version                  |
|:-------------------:|:---------------------------:|
| Ballerina Language  | Swan-Lake-Preview8          |
| Service Bus API     | v1.2.8                      |

# Quickstart(s)

## Send and Receive Messages from the Azure Service Bus Queue

This is the simplest scenario to send and receive messages from an Azure Service Bus queue. You need to obtain 
a connection string of the name space and an entity path name of the queue you want to send and receive messages from. 
You must have the following prerequisites in order to obtain these configurations.

* An Azure account and subscription.
  If you don't have an Azure subscription, [sign up for a free Azure account](https://azure.microsoft.com/free/).

* A Service Bus namespace.
  If you don't have [a service bus namespace](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-create-namespace-portal),
  learn how to create your Service Bus namespace.
  
* Connection String.
  [Get the connection string](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#get-the-connection-string) from the service bus namespace you created.

* Entity Path.
  A messaging entity, in this case a queue. If you don't have these items, learn how to [Create a queue in the Azure portal](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#create-a-queue-in-the-azure-portal) 
  and get the name of the queue you created. The Entity path is in the format ‘queueName’.
  (Note: The Entity path is in the format ‘topicName‘ for a topic and ‘topicName/subscriptions/subscriptionName’ for a 
  subscription.)

### Step 1: Import the Azure Service Bus Ballerina Library
First, import the ballerinax/asb module into the Ballerina project.
```ballerina
    import ballerinax/asb as asb;
```

### Step 2: Initialize the Azure Service Bus Client Configuration
You can now make the connection configuration using the connection string and entity path.
```ballerina
    asb:ConnectionConfiguration config = {
       connectionString: config:getAsString("CONNECTION_STRING"),
       entityPath: config:getAsString("QUEUE_PATH")
    };
```

### Step 3: Create a Sender Connection using the connection configuration
You can now make a sender connection using the connection configuration.
```ballerina
    asb:SenderConnection? senderConnection = checkpanic new (config);
```

### Step 4 : Create a Receiver Connection using the connection configuration
You can now make a receiver connection using the connection configuration.
```ballerina
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);
```

### Step 5: Initialize the Input Values
Initialize the message to be sent with optional parameters and properties.
```ballerina
    string stringContent = "This is My Message Body";
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "text/plain", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2",
       label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
```

### Step 6: Send a Message to Azure Service Bus
You can now send a message to the configured azure service bus entity with message content, optional parameters and 
properties. Here we have shown how to send a text message and a json message parsed to the byte array format.
```ballerina
    if (senderConnection is asb:SenderConnection) {
       log:print("Sending via Asb sender connection.");
       Checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
       checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
       log:printError("Asb sender connection creation failed.");
    }
```

### Step 7: Receive a Message from Azure Service Bus
You can now receive a message from the configured azure service bus entity.
```ballerina
    if (receiverConnection is asb:ReceiverConnection) {
       log:print("Receiving from Asb receiver connection.");
       asb:Message|asb:Error? messageReceived = receiverConnection->receiveMessage(serverWaitTime);
       asb:Message|asb:Error? jsonMessageReceived = receiverConnection->receiveMessage(serverWaitTime);
       if (messageReceived is asb:Message && jsonMessageReceived is asb:Message) {
           string messageRead = checkpanic messageReceived.getTextContent();
           log:print("Reading Received Message : " + messageRead);
           json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
           log:print("Reading Received Message : " + jsonMessageRead.toString());
       }
    }
```

### Step 8: Close Sender Connection
You can now close the sender connection.
```ballerina
    if (senderConnection is asb:SenderConnection) {
       log:print("Closing Asb sender connection.");
       checkpanic senderConnection.closeSenderConnection();
    }
```

### Step 9: Close Receiver Connection
You can now close the receiver connection.
```ballerina
    if (receiverConnection is asb:ReceiverConnection) {
       log:print("Closing Asb receiver connection.");
       checkpanic receiverConnection.closeReceiverConnection();
    }
```

## Send and Listen to Messages from the Azure Service Bus Queue

This is the simplest scenario to listen to messages from an Azure Service Bus queue. You need to obtain a connection 
string of the name space and an entity path name of the queue you want to listen messages from. You must have the 
following prerequisites in order to obtain these configurations.

* An Azure account and subscription.
  If you don't have an Azure subscription, [sign up for a free Azure account](https://azure.microsoft.com/free/).

* A Service Bus namespace.
  If you don't have [a service bus namespace](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-create-namespace-portal),
  learn how to create your Service Bus namespace.

* Connection String.
  [Get the connection string](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#get-the-connection-string) from the service bus namespace you created.

* Entity Path.
  A messaging entity, in this case a queue. If you don't have these items, learn how to [Create a queue in the Azure portal](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal#create-a-queue-in-the-azure-portal)
  and get the name of the queue you created. The Entity path is in the format ‘queueName’.
  (Note: The Entity path is in the format ‘topicName‘ for a topic and ‘topicName/subscriptions/subscriptionName’ for a
  subscription.)

### Step 1: Import the Azure Service Bus Ballerina Library
First, import the ballerinax/asb module into the Ballerina project.
```ballerina
    import ballerinax/asb as asb;
```

### Step 2: Initialize the Azure Service Bus Client Configuration
You can now make the connection configuration using the connection string and entity path.
```ballerina
    asb:ConnectionConfiguration config = {
       connectionString: config:getAsString("CONNECTION_STRING"),
       entityPath: config:getAsString("QUEUE_PATH")
    };
```

### Step 3: Create a Sender Connection using the connection configuration
You can now make a sender connection using the connection configuration.
```ballerina
    asb:SenderConnection? senderConnection = checkpanic new (config);
```

### Step 4: Initialize the Input Values
Initialize the message to be sent with optional parameters and properties.
```ballerina
    string stringContent = "This is My Message Body";
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "text/plain", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2",
       label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
```

### Step 5: Send a Message to Azure Service Bus
You can now send a message to the configured azure service bus entity with message content, optional parameters and
properties. Here we have shown how to send a text message and a json message parsed to the byte array format.
```ballerina
    if (senderConnection is asb:SenderConnection) {
       log:print("Sending via Asb sender connection.");
       Checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
       checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
       log:printError("Asb sender connection creation failed.");
    }
```

### Step 6: Create a service object with the service configuration and the service logic to execute based on the message received
You can now create a service object with the service configuration specified using the @asb:ServiceConfig annotation. 
We need to give the connection string and the entity path of the queue we are to listen messages from. Then we can give 
the service logic to execute when a message is received inside the onMessage remote function.
```ballerina
    asb:Service asyncTestService =
    @asb:ServiceConfig {
        queueConfig: {
            connectionString: config:getAsString("CONNECTION_STRING"),
            queueName: config:getAsString("QUEUE_PATH")
        }
    }
    service object {
        remote function onMessage(asb:Message message) {
            var messageContent = message.getTextContent();
            if (messageContent is string) {
                log:print("The message received: " + messageContent);
            } else {
                log:printError("Error occurred while retrieving the message content.");
            }
        }
    };
```

### Step 7: Initialize the ASB Listener  and Asynchronously listen to messages from the Azure Service Bus Queue
You can now initialize the ASB Listener and attach the service object with the listener. Then the user can start the 
listener and asynchronously listen to messages from the azure service bus connection and execute the service logic 
based on the message received. Here we have sent the current worker to sleep for 20 seconds. You can detach the service 
from the listener endpoint at any instance and stop listening to messages. You can gracefully stop listening by 
detaching from the listener and by terminating the connection. You can also immediately stop listening by terminating 
the connection with the Azure Service Bus.
```ballerina
    asb:Listener? channelListener = new();
    if (channelListener is asb:Listener) {
        checkpanic channelListener.attach(asyncTestService);
        checkpanic channelListener.'start();
        log:print("start listening");
        runtime:sleep(20000);
        log:print("end listening");
        checkpanic channelListener.detach(asyncTestService);
        checkpanic channelListener.gracefulStop();
        checkpanic channelListener.immediateStop();
    }
```

### Step 8: Close Sender Connection
You can now close the sender connection.
```ballerina
    if (senderConnection is asb:SenderConnection) {
       log:print("Closing Asb sender connection.");
       checkpanic senderConnection.closeSenderConnection();
    }
```

# Samples

### Send and Receive Message from Queue
This is the basic scenario of sending and receiving a message from a queue. A user must create a sender connection and 
a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can receive 
the Message object at the other receiver end.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/send_and_receive_message_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "text/plain", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int serverWaitTime = 5;

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection.");
        asb:Message|asb:Error? messageReceived = receiverConnection->receiveMessage(serverWaitTime);
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection->receiveMessage(serverWaitTime);
        if (messageReceived is asb:Message && jsonMessageReceived is asb:Message) {
            string messageRead = checkpanic messageReceived.getTextContent();
            log:print("Reading Received Message : " + messageRead);
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } 
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Send and Receive Messages from Queue
This is the basic scenario of sending and receiving one or more messages from a queue. A user must create a 
sender connection and a receiver connection with the azure service bus to send and receive a message. The message body 
is passed as a parameter in byte array format with optional parameters and properties to the send operation. The user 
can receive the Message object at the other receiver end.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/send_and_receive_messages_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "text/plain", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int maxMessageCount = 3;
    int serverWaitTime = 5;

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection.");
        var messageReceived = receiverConnection->receiveMessages(serverWaitTime, maxMessageCount);
        if(messageReceived is asb:Messages) {
            int val = messageReceived.getMessageCount();
            log:print("No. of messages received : " + val.toString());
            asb:Message[] messages = messageReceived.getMessages();
            string messageReceived1 =  checkpanic messages[0].getTextContent();
            log:print("Message1 content : " +messageReceived1);
            json messageReceived2 =  checkpanic messages[1].getJSONContent();
            log:print("Message2 content : " +messageReceived2.toString());
        } else {
            log:printError(messageReceived.message());
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}  
```

### Send and Receive Batch from Queue
This is the basic scenario of sending and receiving a batch of messages from a queue. A user must create a sender 
connection and a receiver connection with the azure service bus to send and receive a message. The message body is 
passed as a parameter in byte array format with optional parameters and properties to the send operation. The user can 
receive the array of Message objects at the other receiver end.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/send_and_receive_batch_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string[] stringArrayContent = ["apple", "mango", "lemon", "orange"];
    map<string> parameters = {contentType: "text/plain", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int maxMessageCount = 3;
    int serverWaitTime = 5;

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendBatchMessage(stringArrayContent, parameters, properties, maxMessageCount);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection.");
        var messageReceived = receiverConnection->receiveBatchMessage(maxMessageCount);
        if(messageReceived is asb:Messages) {
            int val = messageReceived.getMessageCount();
            log:print("No. of messages received : " + val.toString());
            asb:Message[] messages = messageReceived.getMessages();
            string messageReceived1 =  checkpanic messages[0].getTextContent();
            log:print("Message1 content : " + messageReceived1);
            string messageReceived2 =  checkpanic messages[1].getTextContent();
            log:print("Message2 content : " + messageReceived2);
            string messageReceived3 =  checkpanic messages[2].getTextContent();
            log:print("Message3 content : " + messageReceived3);
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Send to Topic and Receive from Subscription
This is the basic scenario of sending a message to a topic and receiving a message from a subscription. A user must 
create a sender connection and a receiver connection with the azure service bus to send and receive a message. 
The message body is passed as a parameter in byte array format with optional parameters and properties to the send 
operation. The user can receive the Message object at the other receiver end.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/send_to_topic_and_receive_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "text/plain", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int serverWaitTime = 5;

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection 1.");
        asb:Message|asb:Error? messageReceived = receiverConnection1->receiveMessage(serverWaitTime);
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection1->receiveMessage(serverWaitTime);
        if (messageReceived is asb:Message && jsonMessageReceived is asb:Message) {
            string messageRead = checkpanic messageReceived.getTextContent();
            log:print("Reading Received Message : " + messageRead);
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }


    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection 2.");
        asb:Message|asb:Error? messageReceived = receiverConnection2->receiveMessage(serverWaitTime);
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection2->receiveMessage(serverWaitTime);
        if (messageReceived is asb:Message && jsonMessageReceived is asb:Message) {
            string messageRead = checkpanic messageReceived.getTextContent();
            log:print("Reading Received Message : " + messageRead);
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection 3.");
        asb:Message|asb:Error? messageReceived = receiverConnection3->receiveMessage(serverWaitTime);
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection3->receiveMessage(serverWaitTime);
        if (messageReceived is asb:Message && jsonMessageReceived is asb:Message) {
            string messageRead = checkpanic messageReceived.getTextContent();
            log:print("Reading Received Message : " + messageRead);
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

### Send Batch to Topic and Receive from Subscription
This is the basic scenario of sending a batch of messages to a topic and receiving a batch of messages from 
a subscription. A user must create a sender connection and a receiver connection with the azure service bus to send and 
receive a message. The message body is passed as a parameter in byte array format with optional parameters and 
properties to the send operation. The user can receive the array of Message objects at the other receiver end.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/send_batch_to_topic_and_receive_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string[] stringArrayContent = ["apple", "mango", "lemon", "orange"];
    map<string> parameters3 = {contentType: "text/plain", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int maxMessageCount = 3;

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendBatchMessage(stringArrayContent, parameters3, properties, maxMessageCount);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection 1.");
        var messagesReceived = receiverConnection1->receiveBatchMessage(maxMessageCount);
        if(messagesReceived is asb:Messages) {
            int val = messagesReceived.getMessageCount();
            log:print("No. of messages received : " + val.toString());
            asb:Message[] messages = messagesReceived.getMessages();
            string messageReceived1 =  checkpanic messages[0].getTextContent();
            log:print("Message1 content : " + messageReceived1);
            string messageReceived2 =  checkpanic messages[1].getTextContent();
            log:print("Message2 content : " + messageReceived2);
            string messageReceived3 =  checkpanic messages[2].getTextContent();
            log:print("Message3 content : " + messageReceived3);
        }else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection 2.");
        var messagesReceived = receiverConnection2->receiveBatchMessage(maxMessageCount);
        if(messagesReceived is asb:Messages) {
            int val = messagesReceived.getMessageCount();
            log:print("No. of messages received : " + val.toString());
            asb:Message[] messages = messagesReceived.getMessages();
            string messageReceived1 =  checkpanic messages[0].getTextContent();
            log:print("Message1 content : " + messageReceived1);
            string messageReceived2 =  checkpanic messages[1].getTextContent();
            log:print("Message2 content : " + messageReceived2);
            string messageReceived3 =  checkpanic messages[2].getTextContent();
            log:print("Message3 content : " + messageReceived3);
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection 3.");
        var messagesReceived = receiverConnection3->receiveBatchMessage(maxMessageCount);
        if(messagesReceived is asb:Messages) {
            int val = messagesReceived.getMessageCount();
            log:print("No. of messages received : " + val.toString());
            asb:Message[] messages = messagesReceived.getMessages();
            string messageReceived1 =  checkpanic messages[0].getTextContent();
            log:print("Message1 content : " + messageReceived1);
            string messageReceived2 =  checkpanic messages[1].getTextContent();
            log:print("Message2 content : " + messageReceived2);
            string messageReceived3 =  checkpanic messages[2].getTextContent();
            log:print("Message3 content : " + messageReceived3);
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

### Complete Messages from Queue
This is the basic scenario of sending and completing messages from a queue. A user must create a sender connection and 
a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can complete 
all the Messages from the queue.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/complete_messages_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "text/plain", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Completing messages from Asb receiver connection.");
        checkpanic receiverConnection->completeMessages();
        log:print("Done completing messages using their lock tokens.");
        log:print("Completing messages from Asb receiver connection.");
        checkpanic receiverConnection->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}  
```

### Complete Message from Queue
This is the basic scenario of sending and completing a message from a queue. A user must create a sender connection and 
a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can complete 
single Messages from the queue.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/complete_message_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "text/plain", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "sanju", replyTo: "carol", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Completing message from Asb receiver connection.");
        checkpanic receiverConnection->completeOneMessage();
        checkpanic receiverConnection->completeOneMessage();
        checkpanic receiverConnection->completeOneMessage();
        log:print("Done completing a message using its lock token.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Complete Messages from Subscription
This is the basic scenario of sending and completing messages from a subscription. A user must create a sender 
connection and a receiver connection with the azure service bus to send and receive a message. The message body is 
passed as a parameter in byte array format with optional parameters and properties to the send operation. The user can 
complete all the Messages from the subscription.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/complete_messages_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string[] stringArrayContent = ["apple", "mango", "lemon", "orange"];
    map<string> parameters = {contentType: "plain/text", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int maxMessageCount = 3;

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendBatchMessage(stringArrayContent, parameters, properties, maxMessageCount);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Completing messages from Asb receiver connection 1.");
        checkpanic receiverConnection1->completeMessages();
        log:print("Done completing messages using their lock tokens.");
        log:print("Completing messages from Asb receiver connection 1.");
        checkpanic receiverConnection1->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Completing messages from Asb receiver connection 2.");
        checkpanic receiverConnection2->completeMessages();
        log:print("Done completing messages using their lock tokens.");
        log:print("Completing messages from Asb receiver connection 2.");
        checkpanic receiverConnection2->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Completing messages from Asb receiver connection 3.");
        checkpanic receiverConnection3->completeMessages();
        log:print("Done completing messages using their lock tokens.");
        log:print("Completing messages from Asb receiver connection 3.");
        checkpanic receiverConnection3->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

### Complete Message from Subscription
This is the basic scenario of sending and completing a message from a subscription. A user must create a sender 
connection and a receiver connection with the azure service bus to send and receive a message. The message body is 
passed as a parameter in byte array format with optional parameters and properties to the send operation. The user can 
complete single Messages from the subscription.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/complete_message_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string[] stringArrayContent = ["apple", "mango", "lemon", "orange"];
    map<string> parameters = {contentType: "application/text", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int maxMessageCount = 3;

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendBatchMessage(stringArrayContent, parameters, properties, maxMessageCount);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Completing message from Asb receiver connection 1.");
        checkpanic receiverConnection1->completeOneMessage();
        checkpanic receiverConnection1->completeOneMessage();
        checkpanic receiverConnection1->completeOneMessage();
        log:print("Done completing a message using its lock token.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Completing message from Asb receiver connection 2.");
        checkpanic receiverConnection2->completeOneMessage();
        checkpanic receiverConnection2->completeOneMessage();
        checkpanic receiverConnection2->completeOneMessage();
        log:print("Done completing a message using its lock token.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Completing message from Asb receiver connection 3.");
        checkpanic receiverConnection3->completeOneMessage();
        checkpanic receiverConnection3->completeOneMessage();
        checkpanic receiverConnection3->completeOneMessage();
        log:print("Done completing a message using its lock token.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

### Abandon Message from Queue
This is the basic scenario of sending and abandoning a message  from a queue. A user must create a sender connection 
and a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can abandon 
a single Messages & make available again for processing from Queue.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/abandon_message_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "sanju", replyTo: "carol", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("abandoning message from Asb receiver connection.");
        checkpanic receiverConnection->abandonMessage();
        log:print("Done abandoning a message using its lock token.");
        log:print("Completing messages from Asb receiver connection.");
        checkpanic receiverConnection->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Abandon Message from Subscription
This is the basic scenario of sending and abandoning a message  from a subscription. A user must create a sender 
connection and a receiver connection with the azure service bus to send and receive a message. The message body is 
passed as a parameter in byte array format with optional parameters and properties to the send operation. The user can 
abandon a single Messages & make available again for processing from subscription.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/abandon_message_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string[] stringArrayContent = ["apple", "mango", "lemon", "orange"];
    map<string> parameters = {contentType: "application/text", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int maxMessageCount = 3;

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendBatchMessage(stringArrayContent, parameters, properties, maxMessageCount);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("abandoning message from Asb receiver connection 1.");
        checkpanic receiverConnection1->abandonMessage();
        log:print("Done abandoning a message using its lock token.");
        log:print("Completing messages from Asb receiver connection 1.");
        checkpanic receiverConnection1->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("abandoning message from Asb receiver connection 2.");
        checkpanic receiverConnection2->abandonMessage();
        log:print("Done abandoning a message using its lock token.");
        log:print("Completing messages from Asb receiver connection 2.");
        checkpanic receiverConnection2->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("abandoning message from Asb receiver connection 3.");
        checkpanic receiverConnection3->abandonMessage();
        log:print("Done abandoning a message using its lock token.");
        log:print("Completing messages from Asb receiver connection 3.");
        checkpanic receiverConnection3->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

### Asynchronous Consumer
This is the basic scenario of sending and listening to messages from a queue. A user must create a sender connection 
and a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can 
asynchronously listen to messages from the azure service bus connection and execute the service logic based on the 
message received.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/async_consumer.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerina/runtime;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    asb:Service asyncTestService =
    @asb:ServiceConfig {
        queueConfig: {
            connectionString: config:getAsString("CONNECTION_STRING"),
            queueName: config:getAsString("QUEUE_PATH")
        }
    }
    service object {
        remote function onMessage(asb:Message message) {
            var messageContent = message.getTextContent();
            if (messageContent is string) {
                log:print("The message received: " + messageContent);
            } else {
                log:printError("Error occurred while retrieving the message content.");
            }
        }
    };

    asb:Listener? channelListener = new();
    if (channelListener is asb:Listener) {
        checkpanic channelListener.attach(asyncTestService);
        checkpanic channelListener.'start();
        log:print("start listening");
        runtime:sleep(20000);
        log:print("end listening");
        checkpanic channelListener.detach(asyncTestService);
        checkpanic channelListener.gracefulStop();
        checkpanic channelListener.immediateStop();
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }
}    
```

### Duplicate Messages Detection
This is the basic scenario of detecting duplicate messages from a queue. A user must create a sender connection and 
a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can receive 
the Message object at the other receiver end. If a duplicate message is received the receive operation fails.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/duplicate_messages_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    map<string> parameters = {contentType: "plain/text", messageId: "one"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int maxMessageCount = 2;
    int serverWaitTime = 5;

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Receiving from Asb receiver connection.");
        var messageReceived = receiverConnection->receiveMessages(serverWaitTime, maxMessageCount);
        if(messageReceived is asb:Messages) {
            int val = messageReceived.getMessageCount();
            log:print("No. of messages received : " + val.toString());
            asb:Message[] messages = messageReceived.getMessages();
            string messageReceived1 =  checkpanic messages[0].getTextContent();
            log:print("Message1 content : " +messageReceived1);
            string messageReceived2 =  checkpanic messages[1].getTextContent();
            log:print("Message2 content : " +messageReceived2.toString());
        } else {
            log:printError(messageReceived.message());
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Deadletter from Queue
This is the basic scenario of sending and dead lettering a message  from a queue. A user must create a sender 
connection and a receiver connection with the azure service bus to send and receive a message. The message body is 
passed as a parameter in byte array format with optional parameters and properties to the send operation. The user can 
abandon a single Message & moves the message to the Dead-Letter Queue.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/deadletter_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Dead-Letter message from Asb receiver connection.");
        checkpanic receiverConnection->deadLetterMessage("deadLetterReason", "deadLetterErrorDescription");
        log:print("Done Dead-Letter a message using its lock token.");
        log:print("Completing messages from Asb receiver connection.");
        checkpanic receiverConnection->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Deadletter from Subscription
This is the basic scenario of sending and dead lettering a message  from a subscription. A user must create a sender 
connection and a receiver connection with the azure service bus to send and receive a message. The message body is 
passed as a parameter in byte array format with optional parameters and properties to the send operation. The user can 
abandon a single Message & move the message to the Dead-Letter subscription.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/deadletter_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Dead-Letter message from Asb receiver connection 1.");
        checkpanic receiverConnection1->deadLetterMessage("deadLetterReason", "deadLetterErrorDescription");
        log:print("Done Dead-Letter a message using its lock token.");
        log:print("Completing messages from Asb receiver connection 1.");
        checkpanic receiverConnection1->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Dead-Letter message from Asb receiver connection 2.");
        checkpanic receiverConnection2->deadLetterMessage("deadLetterReason", "deadLetterErrorDescription");
        log:print("Done Dead-Letter a message using its lock token.");
        log:print("Completing messages from Asb receiver connection 2.");
        checkpanic receiverConnection2->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Dead-Letter message from Asb receiver connection 3.");
        checkpanic receiverConnection3->deadLetterMessage("deadLetterReason", "deadLetterErrorDescription");
        log:print("Done Dead-Letter a message using its lock token.");
        log:print("Completing messages from Asb receiver connection 3.");
        checkpanic receiverConnection3->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

### Defer from Queue
This is the basic scenario of sending and deferring a message  from a queue. A user must create a sender connection and 
a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can defer 
a single Message. Deferred messages can only be received by using sequence number and return Message object.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/defer_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int serverWaitTime = 5;

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Defer message from Asb receiver connection.");
        var sequenceNumber = receiverConnection->deferMessage();
        log:print("Done Deferring a message using its lock token.");
        log:print("Receiving from Asb receiver connection.");
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection->receiveMessage(serverWaitTime);
        if (jsonMessageReceived is asb:Message) {
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
        log:print("Receiving Deferred Message from Asb receiver connection.");
        if(sequenceNumber is int) {
            if(sequenceNumber == 0) {
                log:printError("No message in the queue");
            }
            asb:Message|asb:Error? messageReceived = receiverConnection->receiveDeferredMessage(sequenceNumber);
            if (messageReceived is asb:Message) {
                string messageRead = checkpanic messageReceived.getTextContent();
                log:print("Reading Received Message : " + messageRead);
            } else if (messageReceived is ()) {
                log:printError("No deferred message received with given sequence number");
            } else {
                log:printError(msg = messageReceived.message());
            }
        } else {
            log:printError(msg = sequenceNumber.message());
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Defer from Subscription
This is the basic scenario of sending and deferring a message  from a subscription. A user must create a sender 
connection and a receiver connection with the azure service bus to send and receive a message. The message body is 
passed as a parameter in byte array format with optional parameters and properties to the send operation. The user can 
defer a single Message. Deferred messages can only be received by using sequence number and return Message object.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/defer_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};
    int serverWaitTime = 5;

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Defer message from Asb receiver connection 1.");
        var sequenceNumber = receiverConnection1->deferMessage();
        log:print("Done Deferring a message using its lock token.");
        log:print("Receiving from Asb receiver connection 1.");
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection1->receiveMessage(serverWaitTime);
        if (jsonMessageReceived is asb:Message) {
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
        log:print("Receiving Deferred Message from Asb receiver connection 1.");
        if(sequenceNumber is int) {
            if(sequenceNumber == 0) {
                log:printError("No message in the queue");
            }
            asb:Message|asb:Error? messageReceived = receiverConnection1->receiveDeferredMessage(sequenceNumber);
            if (messageReceived is asb:Message) {
                string messageRead = checkpanic messageReceived.getTextContent();
                log:print("Reading Received Message : " + messageRead);
            } else if (messageReceived is ()) {
                log:printError("No deferred message received with given sequence number");
            } else {
                log:printError(msg = messageReceived.message());
            }
        } else {
            log:printError(msg = sequenceNumber.message());
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Defer message from Asb receiver connection 2.");
        var sequenceNumber = receiverConnection2->deferMessage();
        log:print("Done Deferring a message using its lock token.");
        log:print("Receiving from Asb receiver connection 2.");
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection2->receiveMessage(serverWaitTime);
        if (jsonMessageReceived is asb:Message) {
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
        log:print("Receiving Deferred Message from Asb receiver connection 2.");
        if(sequenceNumber is int) {
            if(sequenceNumber == 0) {
                log:printError("No message in the queue");
            }
            asb:Message|asb:Error? messageReceived = receiverConnection2->receiveDeferredMessage(sequenceNumber);
            if (messageReceived is asb:Message) {
                string messageRead = checkpanic messageReceived.getTextContent();
                log:print("Reading Received Message : " + messageRead);
            } else if (messageReceived is ()) {
                log:printError("No deferred message received with given sequence number");
            } else {
                log:printError(msg = messageReceived.message());
            }
        } else {
            log:printError(msg = sequenceNumber.message());
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Defer message from Asb receiver connection 3.");
        var sequenceNumber = receiverConnection3->deferMessage();
        log:print("Done Deferring a message using its lock token.");
        log:print("Receiving from Asb receiver connection 3.");
        asb:Message|asb:Error? jsonMessageReceived = receiverConnection3->receiveMessage(serverWaitTime);
        if (jsonMessageReceived is asb:Message) {
            json jsonMessageRead = checkpanic jsonMessageReceived.getJSONContent();
            log:print("Reading Received Message : " + jsonMessageRead.toString());
        } else {
            log:printError("Receiving message via Asb receiver connection failed.");
        }
        log:print("Receiving Deferred Message from Asb receiver connection 3.");
        if(sequenceNumber is int) {
            if(sequenceNumber == 0) {
                log:printError("No message in the queue");
            }
            asb:Message|asb:Error? messageReceived = receiverConnection3->receiveDeferredMessage(sequenceNumber);
            if (messageReceived is asb:Message) {
                string messageRead = checkpanic messageReceived.getTextContent();
                log:print("Reading Received Message : " + messageRead);
            } else if (messageReceived is ()) {
                log:printError("No deferred message received with given sequence number");
            } else {
                log:printError(msg = messageReceived.message());
            }
        } else {
            log:printError(msg = sequenceNumber.message());
        }
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

### Renew Lock on Message from Queue
This is the basic scenario of renewing a lock on a message  from a queue. A user must create a sender connection and 
a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation.  The user can renew 
a lock on a single Message from the queue.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/renew_lock_on_message_from_queue.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration config = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("QUEUE_PATH")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (config);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection = checkpanic new (config);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Renew lock on message from Asb receiver connection.");
        checkpanic receiverConnection->renewLockOnMessage();
        log:print("Done renewing a message.");
        log:print("Completing messages from Asb receiver connection.");
        checkpanic receiverConnection->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection.");
        checkpanic receiverConnection.closeReceiverConnection();
    }
}    
```

### Renew Lock on Message from Subscription
This is the basic scenario of renewing a lock on a message from a subscription. A user must create a sender connection 
and a receiver connection with the azure service bus to send and receive a message. The message body is passed as 
a parameter in byte array format with optional parameters and properties to the send operation. The user can renew 
a lock on a single Message from the subscription.

Sample is available at:
https://github.com/ballerina-platform/module-ballerinax-azure-service-bus/blob/main/asb-ballerina/samples/renew_lock_on_message_from_subscription.bal

```ballerina
import ballerina/config;
import ballerina/log;
import ballerinax/asb;

public function main() {

    // Input values
    string stringContent = "This is My Message Body"; 
    byte[] byteContent = stringContent.toBytes();
    json jsonContent = {name: "apple", color: "red", price: 5.36};
    byte[] byteContentFromJson = jsonContent.toJsonString().toBytes();
    map<string> parameters1 = {contentType: "plain/text", messageId: "one"};
    map<string> parameters2 = {contentType: "application/json", messageId: "two", to: "user1", replyTo: "user2", 
        label: "a1", sessionId: "b1", correlationId: "c1", timeToLive: "2"};
    map<string> properties = {a: "propertyValue1", b: "propertyValue2"};

    asb:ConnectionConfiguration senderConfig = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("TOPIC_PATH")
    };

    asb:ConnectionConfiguration receiverConfig1 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH1")
    };

    asb:ConnectionConfiguration receiverConfig2 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH2")
    };

    asb:ConnectionConfiguration receiverConfig3 = {
        connectionString: config:getAsString("CONNECTION_STRING"),
        entityPath: config:getAsString("SUBSCRIPTION_PATH3")
    };

    log:print("Creating Asb sender connection.");
    asb:SenderConnection? senderConnection = checkpanic new (senderConfig);

    log:print("Creating Asb receiver connection.");
    asb:ReceiverConnection? receiverConnection1 = checkpanic new (receiverConfig1);
    asb:ReceiverConnection? receiverConnection2 = checkpanic new (receiverConfig2);
    asb:ReceiverConnection? receiverConnection3 = checkpanic new (receiverConfig3);

    if (senderConnection is asb:SenderConnection) {
        log:print("Sending via Asb sender connection.");
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContent, parameters1, properties);
        checkpanic senderConnection->sendMessageWithConfigurableParameters(byteContentFromJson, parameters2, properties);
    } else {
        log:printError("Asb sender connection creation failed.");
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Renew lock on message from Asb receiver connection 1.");
        checkpanic receiverConnection1->renewLockOnMessage();
        log:print("Done renewing a message.");
        log:print("Completing messages from Asb receiver connection 1.");
        checkpanic receiverConnection1->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Renew lock on message from Asb receiver connection 2.");
        checkpanic receiverConnection2->renewLockOnMessage();
        log:print("Done renewing a message.");
        log:print("Completing messages from Asb receiver connection 2.");
        checkpanic receiverConnection2->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Renew lock on message from Asb receiver connection 3.");
        checkpanic receiverConnection3->renewLockOnMessage();
        log:print("Done renewing a message.");
        log:print("Completing messages from Asb receiver connection 3.");
        checkpanic receiverConnection3->completeMessages();
        log:print("Done completing messages using their lock tokens.");
    } else {
        log:printError("Asb receiver connection creation failed.");
    }

    if (senderConnection is asb:SenderConnection) {
        log:print("Closing Asb sender connection.");
        checkpanic senderConnection.closeSenderConnection();
    }

    if (receiverConnection1 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 1.");
        checkpanic receiverConnection1.closeReceiverConnection();
    }

    if (receiverConnection2 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 2.");
        checkpanic receiverConnection2.closeReceiverConnection();
    }

    if (receiverConnection3 is asb:ReceiverConnection) {
        log:print("Closing Asb receiver connection 3.");
        checkpanic receiverConnection3.closeReceiverConnection();
    }
}    
```

## Building from the Source

### Setting Up the Prerequisites

1. Download and install Java SE Development Kit (JDK) version 11 (from one of the following locations).

    * [Oracle](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)

    * [OpenJDK](https://adoptopenjdk.net/)

      > **Note:** Set the JAVA_HOME environment variable to the path name of the directory into which you installed JDK.

2. Download and install [Ballerina SLP8](https://ballerina.io/).

### Building the Source

Execute the commands below to build from the source after installing Ballerina SLP8 version.

1. To Build and Package the Native Java Wrapper:
   Change the current directory to the asb-native home directory and execute the following command.
```shell script
    mvn clean package
```

2. To build the Ballerina library:
   Change the current directory to the asb-ballerina home directory and execute the following command.
```shell script
    ballerina build
```

3. To build the module without the tests:
   Change the current directory to the asb-ballerina home directory and execute the following command.
```shell script
    ballerina build --skip-tests
```

## Issues and Projects

Issues and Projects tabs are disabled for this repository as this is part of the Ballerina Standard Library. To report bugs, request new features, start new discussions, view project boards, etc. please visit Ballerina Standard Library [parent repository](https://github.com/ballerina-platform/ballerina-standard-library).

This repository only contains the source code for the module.

## Contributing to Ballerina

As an open source project, Ballerina welcomes contributions from the community.

For more information, go to the [contribution guidelines](https://github.com/ballerina-platform/ballerina-lang/blob/master/CONTRIBUTING.md).

## Code of Conduct

All the contributors are encouraged to read the [Ballerina Code of Conduct](https://ballerina.io/code-of-conduct).

## Useful Links

* Discuss the code changes of the Ballerina project in [ballerina-dev@googlegroups.com](mailto:ballerina-dev@googlegroups.com).
* Chat live with us via our [Slack channel](https://ballerina.io/community/slack/).
* Post all technical questions on Stack Overflow with the [#ballerina](https://stackoverflow.com/questions/tagged/ballerina) tag.
