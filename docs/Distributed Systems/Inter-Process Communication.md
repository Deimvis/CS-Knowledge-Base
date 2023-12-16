# Inter-Process Communication

* Inter-Process Communication (IPC) — mechanisms an operating system or other enviornments provide to allow the processes to manage shared data
* Approaches
  * File
  * Communications file
  * Signal (also Asynchronous System Trap)
  * Socket
  * Unix domain socket
  * Message queue
  * Anonymous pipe
  * Named pipe
  * Shared memory
  * Message passing
  * Memory-mapped file

## Message Passing

* Message Passing — technique for invoking behavior on a computer through sending messages
* Model
  * One-Way
  * Request-Reply
* Way
  * Push
  * Pull
* Coupling
  * Direct communication — communication when processses know about each other
  * Indirect communicaiton — communication when processes don't know about each other, and it's performed via an intermediary
    * Space uncoupling: processes doesn't know each other's identity
    * Time uncoupling:  processes have independent lifetimes
* Blocking
  * Synchronous communication: process perform operations related to communication blocking itself
  * Asynchronous communication: process perofrm operations related to communication without being blocked
* Channel
  * Reliable
  * Unreliable

## Direct Communication

### API

* API (Application Programming Interface) — a type of software interface, offering a service to other pieces of software

#### REST API

* REST API (Representational State Transfer API) — a software architectural style that was created to guide the design and development of the architecture for the World Wide Web

* Benefits

  * Scalable
  * Uniform Interface
  * Cachable
  * Flexible (to change inner implementation)
  * Compatible
  * Simple to use

* Drawbacks

  * Lack of state
  * Security (REST doesn't impose security)

* Best Practices

  * Organized around resources ('/orders' + POST instead of '/view-order' or '/create-order')
  * Entities are grouped into collecitons ('/orders' instead of '/order/all')
  * Parametrized URLs for identity ('/orders/{order_id}' instead of '/orders?order_id=1')
  * Keep URLs simple ('/orders' instead of '/user/{user_id}/orders')
  * Use query params for additional options or metadata (sort, limit ,etc)
  * Use hyphens in URLs instead of underscores or anything else ('/video-content' instead of 'video_content', 'videocontent')

* Versioning

  | Method           | Example                 | Cache-Friendly | RESTFUL |
  | ---------------- | ----------------------- | -------------- | ------- |
  | via URI          | '/v2/...                | YES            | NO      |
  | via query params | ...?version=2           | YES            | NO      |
  | via headers      | api-verison=2           | NO             | YES     |
  | via media type   | application/xxx.v2+json | NO             | YES     |

#### RPC API

* RPC (Remote Procedure Call) — an event when process causes a procedure to execute in a different address space (commonly on another computer on a shared network), which is coded as if it were a normal (local) procedure call, without the programmer explicitly coding the details for the remote interaction

## Indirect communication

* Indirect communication — communication through a broker or an abstraction without direct coupling sender and recipient by messaging
  * Types of coupling
    * space coupling: processes have information about each other
    * time coupling: processes run together
* Types
  * Transient communication: nodes do not store message, message is dropped if recipient is unavailable
  * Persistent communication: messages is stored until recipient receive it
* Models
  * Group communication
  * Message Queue
  * Publish-Subscribe
  * Shared Memory

### Group Communication

* Group Communication — communicaiton with the participation of the group of processes
* Group Structure
  * Egalitarian: decisions are determined collectively
  * Hierarchical: decisions are determined by the coordinator 
* Types
  * Unicast: sender communicates with a single process in system
  * Multicast: sender communicates with a designated group of processes in system
  * Broadcast: sender communicates with every process in system
  * Anycast: sender communicates with one random chosen process in system
* Ordering
  * No order: messages are delivered in unknown order
  * FIFO: messages are delivered in the order in which they were sent from specific member
  * Causal: messages are delivered when all messages, which were recived by sender, are received by receiver
  * Total: messages are delivered in the same order to every group member

#### Multicast

##### Basic Multicast

* Basic Multicast (aka best-effort multicast) — network protocol that provides sequence of packets to multiple recipients simultaneously with weak guarantees, commonly uses point-to-point channel
* Properties
  * No Duplication (working process delivers $m$ no more than once)
  * No Creation (if working process deliver $m$ then $m$ was multicast by some process)
  * Validity (if working process multicast $m$ then $m$ will be eventually delivered by every working process)

##### Reliable Multicast

* Reliable multicast — network protocol that provides a reliable sequence of packets to multiple recipients simultaneously
* Properties
  * No Duplication (working process delivers $m$ no more than once)
  * No Creation (if working process deliver $m$ then $m$ was multicast by some process)
  * Validity (if working process multicast $m$ then $m$ will be eventually delivered by every working process)
  * Agreement (if a working process delivers $m$ then all group working processes in group $g$ will deliver $m$)
* Additional Properties
  * Uniform Agreement (if a process delivers $m$ then all group working processes in group $g$ will deliver $m$)
* Implementations
  * Eager Reliable Multicast
  * Gossip
  * Lazy approach + Failure detector
  * IP Multicast + Acknowledges

###### Eager Reliable Multicast

* Eager Reliable Multicast — protocol with peer-to-peer communication in which every message receiver disseminate it to others

* Complexity: $O(N^2)$ messages

###### Gossip

* Gossip — protocol with peer-to-peer communication that is based on the way epidemics spread (commonly every message receiver disseminate it to other $k$ random nodes every round)
* Complexity: $O(\log N)$ rounds
* Model
  * Push (nodes disseminate information)
  * Pull (nodes ask for information)
  * Push+Pull (nodes do both)
* Modifications
  * Node stop disseminating information with probability $p_{stop}$ if other node to which it was sent already has it

#### Overlay Network

* Overelay Network — virtual or logical network that is created on top of an existing physical network
* Topology
  * Star
  * Mesh
  * Full Mesh
  * Bus
  * Linear
  * Ring
  * Tree

### Message Queue

* Message Queue — software components used for inter-process communication (IPC) providing asynchronous communication protocols to allow sender and receiver to communicate remotely and at different times
* Benefits
  * +Resource Utilization (no component in the system is ever stalled waiting for another, optimizing data flow)
  * +Reliability (if one side failed, other side can continue working)
  * +Flexibility (scale on demand)
  * +Simplicity (no need to design communication)
* Drawbacks
  * +Latency
  * +Deployment
  * +Cost
* Features
  * Durability (store messages in memory / disk / DBMS)
  * Security Policies (access to messages)
  * Message Purging Policies (message's TTL)
  * Message Filtering (subcriber may only see messages matching some criteria)
  * Delivery Policies (at least once or another guarantee)
  * Routing Policies (which queues should receive message, which servers should receive queue's message)
  * Batching Policies (deliver message immediately or wait and deliver many at once)
  * Queuing Criteria (when message is considered "enqueued")
  * Receipt Notification (publisher may know whether subscriber received message or not)
* Usage Examples
  * Asynchronous message exchanging
  * Dispensing tasks between workers
* Examples
  * Enterprise
    * IBM MQ, Java Message Service (JMS)
  * Advanced Message Queuing Protocol (AMQP)
    * RabbitMQ, Apache ActiveMQ, Apache Qpid
  * Task/Job Queues
    * Gearman, Redis
  * Cloud Services
    * Amazon Simple Queue Service, Yandex Message Queue

#### AMQP

* AMQP (Advanced Mesaged Queuing Protocol) — an open standard application layer protocol for message-oriented middleware
* Features
  * Targeted QoS
  * Persistence
  * Multiple consumers
  * High speed

##### Specifications

[AMQP Specifications](https://www.rabbitmq.com/protocol.html)

###### Frame

* Types
  * Method frame
  * Content Header frame
  * Body frame
  * Heartbeat frame

* Structure

  1. Frame Type

  2. Channel #

  3. Size

  4. Payload
     1. Class
     2. Method

  5. End-byte marker

#### Apache Kafka

* Apache Kafka — a distributed event store and stream-processing platform

* Benefits

  * Scalability (using partitions)
  * Low Latency (upto 10ms)
  * High Throughput (>1000 messages/s)
  * Fault Tolerance (messages are stored on disk)
  * Durability (replication feature)
  * Simple Integration (each consumer/producer should be aware of Kafka and not about other participants of the system)
  * Batch Approach, Can Work as ETL

* Drawbacks

  * No complete set of monitoring and managing tools
  * No support for balancing data across nodes (uneven record distribution)
  * No inflight modification of messages
  * No point-point or request/reply paradigms

* Features

  * Streams API (processing data inside kafka, e.g. grouping - group by)
  * Consumer's offsets for each partition are stored in special topic called Consumer offset
  * Retention Policy (globally or per topic)
  * Delivery Guarantees (At most once, At lease once, Exactly once; Exactly once is actually impossible, but with this guarantee you process each message only once)
  * Consumer groups + auto load balancing + auto rebalance (it's a single consumer for Kafka but having multiple nodes to distribute load, so Kafka assigns different partitions to different topics)
  * Compacted topics (for each key store only last received value)
  * Security features (optional)

* Problems

  * Uneven record distribution (Kakfa doesn't aware of partitions' sizes, so you need to balance data by yourself)

    [Challenge of uneven record distribution](https://aiven.io/developer/balance-data-across-kafka-partitions#challenge-of-uneven-record-distribution)

    * Solution:
      * Default partitioning: change settings for "linger time" and a maximum size of the batch
      * Partitioning by key: no off-the-shelf approach

##### Architecture

<img src="/Users/dbrusenin/Knowledge/Global/Computer Science/Distributed Systems/images/kafka_architecture.png" style="zoom:70%">

##### Stream Processing

Approahces from the most simple to the mose flexible

1. KSQL (ksqlDB)
2. Kafka Streams API
3. Producer/Consumer

#### RabbitMQ

* RabbitMQ — an open-source message-broker software (sometimes called message-oriented middleware) that originally implemented the Advanced Message Queuing Protocol (AMQP) and has since been extended with a plug-in architecture to support Streaming Text Oriented Message Protocol (STOMP), MQ Telemetry Transport (MQTT), and other protocols
* Benefits
  * Flexibility in Messaging Patterns and Routing
  * Reliability
  * Durability
  * Scalability
  * Simplicity
  * Security
  * Monitoring UI

* Drawbacks
  * Higher Latency
  * Resource Intensity
  * Difficult to Scale, Complex Configuration (for cluster)
  * Difficult to Preserve Message Ordering Guarantees

* Features
  * Supports request/reply paradigm
  * Flexible routing (exchange to exchange routing)


##### Architecture

<img src="/Users/dbrusenin/Knowledge/Global/Computer Science/Distributed Systems/images/rabbitmq_architecture.png">

##### Exchange

* Exchange — RabbitMQ's component that receives messages from producers and pushes them to corresponding queues

* Types
  * Default (uses queue name)
  * Direct (uses exact match)
  * Topic (uses wildcards)
  * Consistent Hashing (distributes evenly)
  * Headers (uses headers)
  * Alternate (used for unrouted messages from previous exchange)
  * Dead letter (used for "dead-lettered" messages)

#### Apache Kafka vs RabbitMQ

| Aspect               | Apache Kafka                                                 | RabbitMQ                                         |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------ |
| **Type**             | Distributed Streaming Platform                               | Message Broker                                   |
| **Message Model**    | Publish-Subscribe, Log Storage                               | Message Queues, Publish-Subscribe, Request/Reply |
| **Performance**      | High throughput and low latency                              | Good throughput, may have higher latency         |
| **Persistence**      | Supports disk-based retention for messages                   | Messages can be persisted to disk                |
| **Scalability**      | Designed for horizontal scalability                          | Scales horizontally, but may require more nodes  |
| **Protocol Support** | Supports Kafka Protocol (binary)                             | Supports multiple protocols (AMQP, MQTT, etc.)   |
| **Security**         | SSL/TLS, SASL, ACLs, Integration with external authentication providers | SSL/TLS, LDAP, OAuth, Custom Plugins             |
| **Management UI**    | Third-party tools like Confluent Control Center              | Management UI available                          |
| **Ecosystem**        | Kafka Streams, Connect, ksqlDB                               | Wide range of plugins and extensions             |
| **Fault Tolerance**  | Data replication, leader-follower model                      | Mirroring, HA queues, Clustering                 |
| **Monitoring**       | JMX metrics, third-party monitoring tools                    | Management and monitoring plugins available      |



### Publish-Subscribe

* Publish-Subscribe — messaging pattern where senders of messages, called publishers, do not program the messages to be sent directly to specific receivers, called subscribers, but instead categorize published messages into classes without knowledge of which subscribers, if any, there may be. Similarly, subscribers express interest in one or more classes and only receive messages that are of interest, without knowledge of which publishers, if any, there are
* Usage examples
  * Financial Systems
  * Collaborative Editing
  * Monitoring
  * Asynchronous message exchanging

* Filtering Options
  * Channel-based
  * Subject-based (metadata inside message)
  * Content-based
  * Type-based (type of events)
  * Specific message
  * Context (e.g. messages from the same location)
  * Complex event processing

### Shared Memory

* Shared Memory — memory that may be simultaneously accessed by multiple programs with an intent to provide communication among them or avoid reduntant copies

#### Distributed Shared Memory

* Distributed Shared Memory — form of memory architecture where physically separated memories can be addressed as a single shared address space
* Benefits
  * +Speed (relative to other IPC methods)
  * User-Friendly interface for developer (arguably)
* Drawbacks
  * -Scalability
  * Low Isolation (of processes)
  * Require Synchronization Primitives
  * Not Portable
  * Hidden Overhead Cost
  * False Sharing
  * Thrashing


