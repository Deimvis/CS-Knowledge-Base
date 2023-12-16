# Scaling

* Scaling — process of increasing scalability
* Scalability — property of a system to handle a growing amount of work by adding resources to the system
* Types
  * Vertical Scaling — process of enhacing system's performance by upgrading the capacity of a single machine
    * Simple implementation, but expensive and limited

  * Horizontal Scaling — process of enhancing system's performance by adding more machines
    * Complex implementation, but cheaper, more efficient and fault-tolerant


## Reverse Proxy

* Reverse Proxy — web server that centralizes internal services and provides unified interfaces to the public
* Benefits
  * +Security (hide information about backend servers, blacklist IPs, limit number of connections per client)
  * +Scalability (clients only see the reverse proxy's IP, allowing scaling servers or changing their configuration)
  * +Flexibility (clients only see the reverse proxy's IP, allowing scaling servers or changing their configuration)
* Drawbacks
  * +Latency
  * +Complexity
  * Single point of failure
* Additional features
  * SSL termination (decrypt incoming requests and encrypt responses, no need to have X.509 certificate on each server)
  * Compression
  * Caching
  * Serving static content

## Service Replication

* Service Replication — approach of running multiple instances of a single service

### Fail-Over

* Fail-Over — approach when service can survive failure of nodes by replacing them with anothers

#### Active-Passive Fail-Over

_aka master-slave failover_

* Active-Passive Fail-Over — approach when passive server on stanby checks if active server is available and in case of failure it takes over its IP address and resumes service

#### Active-Active Fail-Over

_aka master-master failover_

* Active-Active Fail-Over — approach when all servers are managing traffic, spreading the load between them

### Load Balancing

* Load Balancing — process of distributing a set of tasks over a set of resources, with the aim of making their overall processing more efficient
* Benefits
  * Increased Read/Write Throughput
  * Fault-Tolerance
* Drawbacks
  * Complexity
  * Overhead
  * (Dynamic Algorithms) Increased Network Load

* Implementation
  * Hardware
  * Software
  * via DNS
  * Client-Side

* Types
  * Layer 4 (connection/session)
  * Layer 7 (application)

#### Algorithms

* Types
  * Static — algorithms that do _not_ take into account the state of different machines
  * Dynamic —  algorithms that take into account the state of different machines

##### Static

* Random
* Round Robin
* Hash
* Power of Two Choices

##### Dynamic

* Least Connections
* Least Time

##### via DNS

* Round-robin DNS
* DNS delegation

## Caching

* Benefits

  * Increased Read Throughput
  * Decreased CPU/GPU Load
  * Decreased Network Load

* Drawbacks

  * Complexity (cache invalidation)
  * Update Delay
  * Expensive

* Implementations

  * Client-Side
  * Intermediate Caching Proxy-Servers
  * CDN (Content Delivery Network)
  * Caching HTTP Reverse Proxy (Nginx, Varnish)
  * Caching Storage (Redis, memcached)

* Algorithms

  * LRU (Least Recently Used)
  * LFU (Least Frequently Used)
  * LFRU (Least Frequently Recently Used)
  * FIrst In First Out

  According to Twitter: <a href="https://www.usenix.org/conference/osdi20/presentation/yang">FIFO works the best for a large number of workloads</a>

  According to research: <a href="http://www.sce.carleton.ca/courses/sysc-5801/chlung/own%20papers/LargeFileCaching.pdf">LRU performs the best based on the data collected in our experiments (Experiments of Large File Caching and Comparisons of Caching Algorithms)</a>

* Cache Invalidation: TODO:

## Sharding

* Sharding — hirozontol partitioning of data in a database or search engine
* Benefits
  * Increased Read/Write Throughput
  * Increased Storage Capacity
  * High Availability
* Drawbacks
  * Complexity
  * Expensive
  * (Geosharding) Susceptible to performance degradation due to excessive network traffic
  * (DB) More expensive JOIN operations
* Use Cases
  * Database
  * Search Engine
  * Cache

### Vertical Sharding

* Vertical Sharding — sharding that splits data by column sets

### Horizontal Sharding

* Horizontal Sharding — sharding that splits data by row sets

* Algorithms
  * Ranged/Dynamic Sharding — sharding that chooses appropriate shard depending on value of record's field using value ranges
  * Algorithmic/Hashed Sharding — sharding that chooses appropriate shard depending on output of algorithm or hash function
  * Entity-/Relationship-Based Sharding — sharding that chooses appropriate shard depending on record's relationships (e.g. user payment transactions would be stored together)
  * Geography-Based Sharding (Geosharding) — sharding that chooses appropriate shard depending on record's geography

## Database Index

* Database Index — data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure

## Message Queue

<a href="#Message Queue">Message Queue</a>

## Parallel Computing

* Parallel Computing

* Classes
  * Single Instruction, Multiple Data (SIMD)
    * Examples: SIMD CPU's instructions, GPU
  * Multiple Instruction, Multiple Data(MIMD)
    * Examples: Systems with shared memory, Systems with distributed memory, Hybrid systems
* Systems Examples:
  * Massively Parallel Processing (MPP)
  * Beowulf Cluster
  * HPC cluster

### Map-Reduce

* Map-Reduce — programming paradigm that enables massive scalability across hundreds or thousands of servers in a Hadoop cluster
* Benefits
  * +Simplicity
  * +Efficiency (CPU Utilization, Performance)
* Drawbacks
  * Not every task can be described as a Map-Reduce job
  * Not efficient for a small dataset / Big overhead relative to a small dataset
* Implementation Examples:
  * Google
  * Apache Hadoop
  * Apache Spark
  * YTsaurus

### Scatter/Gather Pattern

* Scatter/Gather Pattern — tree pattern with a root node that distributes requests and leaves that process those requests. Each replica does a small amount of processing and then returns a fraction of the result to the root
* Usage examples
  * Distributed search

### Tail Latency

* Tail Latency — high percentile latency (e.g. latency of requests with response time longer than 99% of all requests)
* Strategies to decrease Tail Latency
  * Degradation (fast response with worse quality)
  * Hedging
  * Tied requests (same request on multiple servers)
  * Additional replicas for hot data
  * Temporary exclusion of slow machines


