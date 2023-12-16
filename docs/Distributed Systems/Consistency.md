# Consistency

* Data Consistency — property of a system to keep data the same at different places

<img src="/Users/dbrusenin/Knowledge/Global/Computer Science/Distributed Systems/images/consistency_models.png" style="zoom:70%">

## Consistency Models

* Consistency model — contract between the programmer and a system, wherein the system guarantees  that if the programmer follows the rules for operations in memory, memory will be consistent and the results of reading, writing, or updating memory will be predictable

### Linearizable

* Linearizable consistency — consistency model that meets requirements of linearizability
* Linearizability — one of the strongest single-object consistency models which implies that every operation appears to take place atomically, in some order, consistent with real-time ordering of those operations
* Linearizability (more formal) — consistency model which provides history H that is equivalent to sequential history S, and the partial real-time order of operations in H is consistent with the total order of S, and which preserves the object's single-threaded semantics
* Benefits
  * Real-time constraints (system works like single-threaded process)
  * +Development Simplicity
* Drawbacks
  * Can't be totally or sticky available
  * In the event of a network partition, some or all nodes will be unable to make progress

### Eventual Consistency

_aka optimistic replication_

* Eventual consisteny — consistency model which informally guarantees that, if no new updates are made to a given data item, eventually all accesses to that data item will return the last updated value
* Benefits
  * +Availability
  * +Performance
  * +Scalability
  * In the event of a network partition any node is able to make progress
* Drawbacks
  * -Development Simplicity
  * +Conflict Resolution
  * -Safety (formally, an eventually consistent system can return any value before it converges)

### Conflict Resolution

* Conflict Resolution — process of reconciliation of differences between multiple copies of distributed data
* Types
  * Read repair (correction is done when a read find inconsistency)
  * Write repair (correction takes place during a write operation)
  * Asynchronous repair (correction is not part of a read or write operation)

### Strong Eventual Consistency

* Strong Eventual consisteny — eventual consistency model with additional safety guarantee: "any two nodes that have received the same (unordered) set of updates will be in the same state"
* Benefits
  * +Availability
  * +Performance
  * +Scalability
  * In the event of a network partition any node is able to make progress
* Drawbacks
  * -Development Simplicity

### Comparison

#### Overall

<img src="/Users/dbrusenin/Knowledge/Global/Computer Science/Distributed Systems/images/consistency_models_comparison.png" style="zoom:50%">

<img src="/Users/dbrusenin/Knowledge/Global/Computer Science/Distributed Systems/images/consistency_models_comparison_2.png" style="zoom:50%">

#### Linearizability vs Serializability

[Linearizability versus Serializability](http://www.bailis.org/blog/linearizability-versus-serializability/)

#### Linearizability vs Sequential Consistency

Linearizability cares about time. Sequential consistency cares about program order. – Should provide the behavior of a single copy – A read operation returns the most recent write, regardless of the clients. – All subsequent read ops should return the same result until the next write, regardless of the clients.

## Clock

* Clock — mechanism for capturing relationships of events in distributed systems

### Clock Drift

* Clock Drift — phenomena where a clock doesn't run at exactly the same rate as a reference clock 

### Physical Clock

* Physical Clock — clock that uses number of elapsed seconds to determine relationships
* Examples
  * Quartz clock
    * Cheap
    * High clock drift (about 10-25 minutes per year)
  * Atomic clock
    * Expensive
    * Small clock drift (about 1 second per 3M years)
    * Used in GPS

### Logical Clock

* Logical Clock — clock that captures chronological and causal relationships

* Algorithms
  * Lamport timestamp
  * Vector clocks
  * Version vectors
  * Matrix clocks

