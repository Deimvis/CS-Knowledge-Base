# Consensus and Related Problems

## Consensus

* Consensus — algorithm allowing nodes of a system to agree on some data value that is needed during computation

* Implementation examples
  * Raft
  * Paxos

## Related Problems

* Examples of real life problems
  * Every user has unique username
  * Account balance shoud be non-negative
  * Number of items in stock should be non-negative
  * Replicated storage should support linearizability
  * Transaction should be completed entirely
* Approaches
  * Single server: locks, atomic operations
  * Distributed system: leader, lock service, transactions coordinator, atomic broadcast, consensus

### Two-Phase Commit (2PC)

_aka two-phase commit protocol_

* Two-Phase Commit — type of atomic commitment protocol
* Benefits
  * Atomicity
  * Tolerates process, network node, communication, etc. failures (temporary)
* Drawbacks
  * Don't tolerate infinite failure (if node crashes forever)
  * Requires stable storage at each node with write-ahead log
    * Stable storage — storage that guarantees atomicity for any given write operation and allows software to be written that is robust against some hardware and power failures
  * Network communication should support rerouting

## Paxos

* Paxos — a family of protocols for solving consensus in a network of unreliable or failible processes

* Benefits
  * Throughput (~1000 tx/s)
  * Latency (~1s/tx)
  * Energy usage
  * Scalable
  * Decentralized
  * Flexible (can be used in various distributed systems)
  * Industry proven
* Drawbacks
  * Difficult to implement
  * Lot of Communication (require many rounds of communication)
* Assumptions
  * Processors
    * Operate at arbitrary speed
    * May experience failures
    * May re-join the protocol after failures (in case of stable storage)
    * Do not collude, lie, etc (no byzantine failures)
  * Network
    * Processors can send messages to each other
    * Messages are sent asynchronously and may take arbitrary time to deliver
    * Messages may be lost, reordered, duplicated
    * Messages are delivered without corruption (no byzantine failures)

### Byzantine Paxos

* Byzantine Paxos — extension of Paxos which supports byzantine failures (arbitrary failures of the participants , including lying, fabrication os messages, collusion with other participants, selective non-participation, etc)

## Raft

* Raft — a consensus algorithm designed as an alternative to the Paxos family of algorithms

* Benefits
  * Throughput (~1000 tx/s)
  * Latency (~1s/tx)
  * Energy usage
  * Simple implementation
  * Single Round of Communication
  * Clear Leader (reduces chances of inconsistencies)
  * Flexible (modular design)
* Drawbacks
  * Scalability (limited)
  * Performance Overhead
  * Leader bottleneck
  * No liveness guarantee (if leader becomes unavailable)

## Blockchain

#### Proof of Work

* Proof of Work (PoW) — a consensus algorithm which is based on the proving that a certain amount of a specific computational effort has been expended

* Benefits
  * Public
  * Relatively Safe
  * Decentralized
* Drawbacks
  * Throughput (~3-7 tx/s)
  * Latency (~3600s/tx)
  * Energy usage (especially bitcoin)
* Implementations
  * Bitcoin

## Byzantine Fault Tolerance

* Byzantine fault — condition of a computer system, particularly distributed computing systems, where components may fail and there is imperfect information on whether a component has failed

### Algorithm

#### Properties

* $f$ – number of traitors

* $n$ – number of nodes

* $n \ge 3f+1$
* $f+1$ rounds
* $O(N^{f+1})$ messages
* Susceptible to attack to certain nodes (e.g. DoS)

### Algorithm (with signatures)

#### Properties

* $f$ – number of traitors

* $n$ – number of nodes

* $n \ge f+2$
* $f+1$ rounds
* $O(N^2)$ messages

### Practical Byzantine Fault Tolerance

#### Properties

* $f$ – number of traitors

* $n$ – number of nodes

* $n \ge 3f+1$
  * If client receives $n-f$ responses then in an assumption of $f$ slow replicas and $f$ traitors' responses to make the decision of majority we need number of trusted responses $(n-f) - f$ be more than number of traitors $f$, so $n \ge 3f$
* $f+1$ rounds
* $O(N^2)$ messages

#### Phases

_case of 4 generals_

1. Pre-prepare
   * The leader is responsible for receiving the Byzantine cilent's attack/retreat command (request)
   * The leader is responsible for initiating the proposal. The said proposal sent to the replicas shall include the message content (attack or retreat), the view number (correponding uniquely to the leader) and a sequence number, which can be described as the numeral order of the action being undertaken
   * The leader sends the "pre-prepare" messages with his signature to other validators via the messenger (communication protocol)

<img src="/Users/dbrusenin/Knowledge/Global/Computer Science/Distributed Systems/images/pbft_preprepare_phase.webp" style="zoom:70%">

2. Prepare
   * After each replica receives the "pre-prepare" message, it can either accept or reject the leader's proposal. If the replica accepts the leader's proposal, it will send a "prepare" message with its own signature to all the other replicas (including the leader). If it rejects, it will not send any message
   * The general who issued the "prepare" message enters the "prepare" phase
   * If a replica receives more than $2f+1$ "prepare" messages, it enters the prepared state. The collection of these prepare messages is collectively referred to as the "prepared certificate"

<img src="/Users/dbrusenin/Knowledge/Global/Computer Science/Distributed Systems/images/pbft_prepare_phase.webp" style="zoom:70%">

3. Commit
   * If the "prepared" general decides to commit, it will send a "commit" message with the signature to all the generals (replicas). If it decides not to execute, no message is sent
   * The general who issued the "commit" message enters the "commit" phase
   * If the generals receive more than $2f+1$ "commit" messages, the message object is executed, which also means that the proposal has reached a consensus
   * After the execution of the message, the general enters the "commited" state and returns the execution result to the Byzantine client
   * After the reply has been sent, the replicas wait for the next request

#### Leader Replacement

* Each general starts a timer after receiving the "prepare" message, a mechanism that is turned off after switching to the "committed" state
* If the message is not executed within a given maximum amount of time T after the timer has been started, the generals broadcast a "view change" message. This message shall include the new view (current view number +1), and a number of other details
* If the leader fails to broadcast the client's request and start the "prepare" phase, each honest replica will eventually send a view change message upon expiration of the timer
* Upon receiving more than $2f+1$ "view change" messages, the new leader can broadcast a "new view" message to all the replicas. This message shall include the new view number, a proof that the previous request has not been executed, a "prepare" message, and a number of other components
* After receiving the "new view" message, each replica shall follow the three steps of request execution described above, once the new leader broadcasts the client's request
* The new leader is now responsible for broadcasting requests from the Byzantine client

### Applications

* Permissioned (private) blockchains (Hyperledger Sawtooth)
* Protocols (Zyzzyva, Tendermint, HotStuff, LibraBFT)


