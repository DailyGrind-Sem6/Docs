# Research Messaging

## What steps need to be taken in order to establish communication between microservices using messaging in my community platform application?

This main research question is about the communication that is needed between microservices. Since responsiblity is divided over multiple services, in some scenarios it is needed for these services to communicate with each other. This communication can be done in multiple ways, one of them is messaging. 

This research will focus on the steps that need to be taken in order to establish communication between microservices using messaging.

## Which messaging brokers are available for microservices and which one is the best fit for my project?

### What is a messaging broker?
Before I look into which messaging brokers are available, I need to know what a messaging broker exactly is. I found a good explanation in an article on medium.com [[1]](#references):

> A message broker is an intermediary program that applications and services use to communicate with each other to exchange information. Message brokers can be used to validate, store, route and deliver messages to the required destinations.

This paragraph explains that a messaging broker is a program that is used to communicate between applications and services. It can not only be used to deliver messages, but also to store them, as mentioned in the paragraph.

The article explains that the point of storing messages is to ensure that the message is not lost in case of a failure. The article explains it further under a section called `So how do message brokers ensure the delivery of the messages?`.

> Message queue stores messages in the exact order they are received and sends to the receiver in the same order. If somehow a message was unable to be delivered (problem with the network) the message will be rescheduled for later in the queue. Also in a message queue, messages are stored in the exact order in which they were transmitted and remain in the queue until receipt is confirmed.

Meaning that the message broker will reschedule a message if it was unable to deliver it. This is a good feature to have when thinking about communication between microservices, because it ensures that the message is not lost.

The final part I want to look at is the components of a message broker. The article explains that a message broker consists of three main components:

> - Producer: This component is responsible for sending messages. It’s connected to the message broker. In publish/subscribe pattern (We have discussed in the below section) they are called publishers.
> 
> - Consumer: This component consumes messages in the message broker. In publish/subscribe pattern they are called subscribers.
> 
> - Queue/topic: Message broker store messages here.

That means a system would be the producer, meaning it sends messages to the message broker. The message broker then stores the messages in a queue or topic. Then finally the consumer, which is the system that receives the messages from the message broker and processes them.

The same article mentions two message brokers that are available for microservices: RabbitMQ and Kafka. These two are mentioned more often in articles about messaging brokers, so I will look into these two.

Although they both have the same goal, they are a bit different in how they work. I found an article on the AWS website that explains the differences between RabbitMQ and Kafka [[2]](#references).

### Architecure

#### RabbitMQ

The article mentions some components of RabbitMQ:

> - An exchange receives messages from the producer and determines where they should be routed to
> 
> - A queue is storage that receives messages from an exchange and sends them to consumers
> 
> - A binding is a path that connects an exchange and a broker

This explains that RabbitMQ uses something called an exchange to receive messages from the producer and determine where they should be routed to. It then mentions the queue, which is the storage that constains messagse and sends them to consumers. Finally, it mentions the binding, which is the path that connects the exchange and the broker.

#### Kafka

The article also mentions some components of Kafka:

> - A Kafka broker is a Kafka server that allows producers to stream data to consumers. The Kafka broker contains topics and their respective partitions
> 
> - A topic is data storage that groups similar data in a Kafka broker.
> 
> - A partition is smaller data storage within a topic that consumers subscribe to.
> 
> - ZooKeeper is special software that manages the Kafka clusters and partitions to provide fault-tolerant streaming. ZooKeeper has recently been replaced with the Apache Kafka Raft (KRaft) protocol.

So here it explains the components of Kafka. This shows that they both have different components, as the article mentions that Kafka consists of a Kafka broker, topics, partitions and ZooKeeper.

### Other Key Differences: Kafka vs. RabbitMQ

#### Performance
Kafka offers superior message transmission capacity, capable of sending millions of messages per second due to its use of sequential disk I/O, which allows for high-throughput message exchange.

In contrast, RabbitMQ can also handle millions of messages per second but typically achieves this only with multiple brokers. Generally, RabbitMQ averages thousands of messages per second and may slow down if its queues become congested.

#### Security
Both RabbitMQ and Kafka provide secure messaging, but they use different technologies. RabbitMQ includes administrative tools to manage user permissions and broker security.

Kafka, on the other hand, secures event streams using TLS for encryption and JAAS for authentication and authorization, preventing unauthorized access and eavesdropping.

#### Programming Languages and Protocols
Kafka and RabbitMQ support various programming languages and protocols. Kafka is compatible with Java, Ruby, Python, and Node.js, using a binary protocol over TCP for real-time data streaming. 

RabbitMQ supports a broader range of languages, including Java, Ruby, JavaScript, Go, C, Swift, Spring, Elixir, PHP, and .NET. It primarily uses the Advanced Message Queuing Protocol (AMQP) but also supports legacy protocols like STOMP and MQTT for message routing.

## What are the different messaging patterns used in microservices and which one is applicable in my project?

There are a bunch of different patterns available that can be used in messaging. I found a page on the Microsoft website which mentions some, I'll go through the most common ones [[3]](#references).

### Asynchronous Request-Reply pattern

The page for this pattern [[4]](#references) explains that this pattern is used when a client send a request to a service and the service sends a response back. Even though this is not in the context of microservice communication, it is still worth mentioning since it is a common pattern.

The page explains it like this:

> In modern application development, it's normal for client applications — often code running in a web-client (browser) — to depend on remote APIs to provide business logic and compose functionality. These APIs may be directly related to the application or may be shared services provided by a third party. Commonly these API calls take place over the HTTP(S) protocol and follow REST semantics.

This is a pattern I'll mostly be using in my frontend application, although in some cases I might use it in the backend for communication between services.

### Competing Consumers pattern

The page for this pattern [[5]](#references) explains that this pattern is used when multiple consumers are competing for messages from a single queue. This is useful when you want to distribute the workload between multiple consumers. This is because a queue in this case will contain one type of task, and when there are multiple consumers, they can all independently go through the tasks and process them at the same time.

The page explains it like this:

> Enable multiple concurrent consumers to process messages received on the same messaging channel. With multiple concurrent consumers, a system can process multiple messages concurrently to optimize throughput, to improve scalability and availability, and to balance the workload.

Below is a diagram that shows a visual representation of the `Competing Consumers pattern`:

![competing-consumers-diagram.png](competing-consumers-diagram.png)

This diagram shows that `Consumer 2`, `Consumer 3` and `Consumer 4` are all listening to the same queue, which contains the same type of tasks. This means that they all would be processing a seperate task each at the same time. Because of this one can see why all the tasks in the lower queue would be processed faster than the ones in the upper queue.

This is a pattern that is incredibly handy to know of in case one does need it, but in my application there is no functionality that would warrant such a pattern.

### Publisher-Subscriber pattern

The page for this pattern [[6]](#references) explains this pattern as a pattern with which one can anounce events to multiple consumers. This is different from a pattern making use of queues, because in this case the message is not task that is stored in a queue waiting to be processed by one consumer, but a message that notifies the consumers of an event. In a queue, the message is processed by one consumer and removed from the queue.

Like explained in the quote below, in this pattern, the message is not consumed and processed by one consumer. It's rather a signal that notifies multiple services of an event that occured and that each service should start their own process that's linked to this event, which can be for example a process that could include updating a database.

The page explains it like this:

> Enable an application to announce events to multiple interested consumers asynchronously, without coupling the senders to the receivers.

The page continues by giving a quick explanation of what a `message` and an `event` are:

> A message is a packet of data. An event is a message that notifies other components about a change or an action that has taken place.

Below is a diagram that shows a visual representation of the `Publisher-Subscriber pattern`:

![publisher-subscriber-pattern-diagram.png](publisher-subscriber-pattern-diagram.png)

The diagram above shows the flow of this pattern for a single topic. It shows a publisher that publishes a message to the broker under a topic. Then, all the subscribers that are listening to that topic will receive the message.

After they receive this message, they all start their own process independently and make the changes that need to be made. One of these processes could be updating a database, another process could be sending emails to specific users, etc. Each of these processes would happen at the same time, yet independently of each other.

I'll very likely need this pattern, because it simplifies the running of multiple processes after a certain event, as supposed to sending a regular HTTP request to each service. With this pattern, I can just send one message to the broker and keep adding subscribers to the topic as needed. With a regular HTTP request, I would have to send a request to each service, which would be a lot more work and harder to maintain over time as more services are created.

### Saga distributed transactions pattern

The page for this pattern [[7]](#references) explains that this pattern is used to achieve consistency across microservices. This pattern consists of a series of transactions that are executed in a specific order. If one of these transactions fails, the saga will execute a series of compensating transactions to undo the changes that were made by the previous transactions.

The page explains it like this:

> The Saga design pattern is a way to manage data consistency across microservices in distributed transaction scenarios. A saga is a sequence of transactions that updates each service and publishes a message or event to trigger the next transaction step. If a step fails, the saga executes compensating transactions that counteract the preceding transactions.

This may be used when duplicated data across multiple databases needs to be updated. This pattern ensures that the data is consistent across all databases, and when a transaction fails, the changes that were made by the previous transactions are undone.

Below is a diagram that shows a visual representation of the `Saga distributed transactions pattern`:

![saga-pattern-diagram.png](saga-pattern-diagram.png)

As shown in the diagram above, this pattern consists of multiple transactions that are executed by seperate services in a specific order. If one of the transactions fails, the system will execute compensating transactions to undo the changes that were made by the previous transactions.

This is a pattern that could be handy in my application, in case of some data that needs to be consistent across multiple databases. Since there can be some data duplication in order to simplify some processes, this pattern can be used in order to ensure that the data is consistent across all databases. Not yet needed in my application, but possible that I'll need it in the future.

## In which scenarios is the use of messaging warranted?

## Conclusion

## References

- [1] Subhashana, H. (2022, 6 januari). Introduction to Message Brokers - Hasitha Subhashana - Medium. Medium. https://hasithas.medium.com/introduction-to-message-brokers-c4177d2a9fe3
- [2] RabbitMQ vs Kafka - Difference Between Message Queue Systems - AWS. (z.d.). Amazon Web Services, Inc. https://aws.amazon.com/compare/the-difference-between-rabbitmq-and-kafka/
- [3] RobBagby. (2023, 13 april). Messaging patterns - Cloud Design Patterns. Microsoft Learn. https://learn.microsoft.com/en-us/azure/architecture/patterns/category/messaging
- [4] RobBagby. (z.d.). Asynchronous Request-Reply pattern - Azure Architecture Center. Microsoft Learn. https://learn.microsoft.com/en-us/azure/architecture/patterns/async-request-reply
- [5] RobBagby. (z.d.-b). Competing Consumers pattern - Azure Architecture Center. Microsoft Learn. https://learn.microsoft.com/en-us/azure/architecture/patterns/competing-consumers
- [6] RobBagby. (z.d.-c). Publisher-Subscriber pattern - Azure Architecture Center. Microsoft Learn. https://learn.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber
- [7] Martinekuan. (z.d.). Saga pattern - Azure Design Patterns. Microsoft Learn. https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/saga/saga