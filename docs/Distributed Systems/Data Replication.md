# Data Replication

* Data Replication — approach of storing the same data on multiple storage devices

* Benefits

  * +Availability
  * +Reliability
  * +Read/Write Throughput
  * -Latency (in case of geo-distributed replication)

* Drawbacks

  * +Complexity
  * +Cost
  * Database Coupling

* Types

  * By user's request processing

    * Active: every replica process client's request

    * Passive: only one replica process client's request and updates others

  * By time coupling

    * Synchronous: client waits until _all_ replicas to be updated
    * Asynchronous: client waits until _subset_ of replicas to be updated

## Replica

* Replica — node in data replication approach
* Type
  * Active — process all requests from clients
  * Passive — process only some of client requests or do not process at all and receive results from active replica

## Consistency

* Data Consistency — property of a system to keep data the same at different places
* Types
  * Eventual Consistency — consistency is reached eventually
  * Strong Consistency — consistency is always met from the client's point of view

### Consistency Tricks

_in case of asynchronous replication_

#### Reading Your Own Writes

* If user is able to modify only small subset of resources then allow user read those resources from leader and rest of them from replicas (e.g. profile in social media)
* If application can suffer a replica lag then measure, for example, 99% percentile of resource replication lag and use it to switch read opeartions to replicas (e.g. 99% percentile of resource replication lag is 200ms then within 200ms after write operation read from leader and since 200ms have passed read from replicas)
* Keep track of most recent update timestamp on client side and send it as a part of read request. Then ensure that replica from which we are reading store data fresh enough according to provided timestamp

#### Monotonic Reads

* Keep track of most recent update timestamp on client side and send it as a part of read request. Then ensure that replica from which we are reading store data fresh anough according to provided timestamp

## Approaches

### Single Leader Replication

_also called as primary-secondary backup, active/passive, leader-follower or master-slave replications_

* Single Leader Replication — data replication with single leader (active replica)
* Benefits

  * +Read Throughput
* Drawbacks

  * +Latency (synchronous mode)
  * Reading stale data (asynchronous mode)
  * Single point of failure

* Examples
  * PostgreSQL
  * MySQL
  * Oracle
  * MongoDB
  * HBase
  * Kafka

#### Workarounds

* Client stores timestamps for read-after-write consistency
* Sticky routing for monotonic read consistency (clients read from the same replica, if it dies, new replica is chosen for reading)
* Quorum and fencing
  * If a network is partitioned into two subsets, the subset with the majority of nodes remains active while shooting the minority subset (this approach is literally called STONITH — Shoot The Other Node In The Head) by sending out a special signal to power supply controller

### Multi Leader Replication

_also called active/active or multi-master replications_

* Multi Leader Replication — data replication with multiple leaders (active replicas)

* Benefits

  * +Distributed Write Load
  * -Latency (in case of geo-distributed replication)
  * Support offline client
* Drawbacks

  * Require leaders coordination (synchronous mode)
  * Require conflict resolution (asynchronous mode)
  * Write order can be broken

* Examples
  * WANdisco
  * CouchDB
  * Google Docs

### Leaderless Replication

* Leaderless Replication — data replication with no leader (every replica is active)

* Benefits

  * +Failure Handling Simplicity
  * +Availability
* Drawbacks

  * -Consistency Maintenance Simplicity
  * Discrepancies can appear
  * Require conflict resolution

* Examples
  * Dynamo
  * Riak
  * Cassandra
  * Voldemort
* Configuration
  * $N$ — number of nodes, $R$ — read quorum size, $W$ — write quorum size
  * $W + R > N$ — reading only up-to-date records
  * Setups
    * $N$ — odd is preferred (look at Fault-Tolerance section)
    * $W = R = (N + 1) / 2$ — balanced setup
    * $W = N$, $R = 1$ — read optimized setup
  * Fault-Tolerance
    * $W < N$, $R < N$
    * $N = 3$, $W = 2$, $R = 2$ — 1 node can fail
    * $N = 4$, $W = 3$, $R = 2$ — 1 node can fail
    * $N = 5$, $W = 3$, $R = 3$ — 2 nodes can fail
  * Additional mechanisms
    * Read Repair — updating stale data and conflict resolution on read operation
    * Anti-Entropy — background process which compares states from replicas and resolves conflicts
    * Hinted handoff — failed node will be temporarily replaced with another one
    * Sloppy quorum — replacement nodes are counted _multiple_ times to quorum size (operations can be served even if number of available replicas is less than quorum size)
    * Strict quorum — replacement nodes are counted _once_ to quorum size

## Conflict Resolution

### Activating

* On read operation
* On write operation

### Policy

* Last Write Wins (LWW)
* Vector Clocks
  * Vector Clock — data structure used for determining the partial ordering of events in distributed system and detecting causality violations
  * Benefits
    * +Causality
  * Drawbacks
    * -API Simplicity
    * +Memory Usage
  * [Why Vector Clocks Are Hard](https://riak.com/posts/technical/why-vector-clocks-are-hard/)
* Application Specific Procedure (including case of client's interaction)
* Conflict-Free Replication Data Types (CRDT) (when order doesn't matter)

## Q&A

* What are advantages and disadvantages of synchronous replication compared to asynchronous?
  * Advantages
    * It's guaranteed that all replicas store up-to-date data
    * no need for conflict resolution
  * Disadvantages
    * works slowly
    * can't write if one replica disabled
* What is replication strategy for Google Docs?
  * Several leaders
    * one leader – big delay, especially in presence of network partition
    * no leader – big delay, especially if there are some of replicas in other region
  * Asynchronous replication
    * synchronous replication - big delay, especially in presence of network partition
* What are disadvantages of LWW?
  * Clock drift causes wrong results of operations
  * No conflict resolution logic
* What is a difference between vector clock and Lamport Clock?
  * Lamport clock doesn't respect causality
* What is a usage example of vector clocks?
  * Questions and replies in messenger's chats require causality


