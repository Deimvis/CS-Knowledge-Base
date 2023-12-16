# Naming and Search

## Address

* Address — kind of object's name that represents its place in network
* Properties
  * Object can have multiple addresses
  * Address can be changed
  * Address can be assigned to another object
  * Address usually has fixed size
  * Address are human-unfriendly

## ID

* ID — kind of object's name that identifies it

* Properties

  * ID refers to no more than one object
  * Object has no more than one ID
  * ID always refers to single object
  * ID doesn't depend on object's address

  * ID can be human-friendly (e.g. domain)

## Naming Structure

* Naming Structure — way of defining relationships between names
* Types
  * Flat
  * Hierarchical
  * Attribute-Based

## Name Resolution

* Broadcast/Multicast
* Table (_name_,_address_)
  * One responsible node stores table
  * Every node stores table
  * Every node stores partial table

## DNS

### Name Resolution

* Iterative
* Recursive
  * Benefits
    * Caching (DNS server caches results)
    * Locality (DNS servers may stay far from user but close to each other)
  * Drawbacks
    * Root DNS Server's Load
* Hybrid (is used in real)
  * Recursive query to local DNS server (usually owned by provider)
  * Iterative query from local DNS server to root and other DNS servers

### Caching

* Server stores DNS records that was received from other servers

### Replication

* DNS zone has primary and secondary servers

## DHT

* DHT (Distributed Hash Table) — distributed system that provides a lookup service similar to a hash table: any participating node can efficiently retrieve the value associated with a given key
* Benefits
  * Nodes can be added or removed with minimum work around re-distributing keys

### Chord

* Chord — protocol for a peer-to-peer DHT
* Complexity
  * Space
    * $O(\log N)$ for every node
  * Time
    * $O(\log N)$ — key' search
    * $O(\log^2 N)$ — keys rearrangement on adding/removing nodes

## DNS vs DHT

* DNS

  * Nodes' responsibles are companies

  * Hierarchical structure
  * Caching

* DHT

  * Nodes' are peers
  * More resistant to attacks (because nodes are peers)
