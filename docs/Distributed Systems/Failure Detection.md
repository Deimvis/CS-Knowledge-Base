# Failure Detection

## Definitions

* Fault — malfunction in system's component that is the reason of error
* Error — problem with system's component inner state that can lead to failure
* Failure — externally visible system's problem
* Fault-tolerance — system's property to continue operating properly even in the presence of faults within some of its components

## Fault

* Reasons
  * Hardware
  * Network
  * Software
  * External Factors
* Types
  * Temporary
  * Periodic
  * Permanent

## Failure

* Types
  * Crash — system stopped working
    * Crash-Stop
    * Crash-Recovery
  * Omission — system skips some of actions
  * Timing — system breaks the working time guarantees
  * Response — system responds incorrectly

## Failure Detector

* Failure Detector — component that monitors processes' status
* Metrics
  * Completeness — share of failed processes that are suspected
    * Strong — every failed process should be suspected by _every_ failure detector
    * Weak — every failed process should be suspected by _some_ failure detector
  * Accuracy — share of unsuspected correct processes
    * Strong — no correct process is suspected
    * Weak — some correct process is never suspected
    * Eventual — strong or weak accuracy eventually
  * Detection Time — metrics on distribution of failure detection's time
  * Load
  * Network Load

## Approaches

### Simple

* Algorithm
  * Periodically ping every process
  * Suspect process if it didn't respond within the time $T_{suspect}$ after ping
  * If afterward suspected process respond on ping then it becomes unsuspected

### Heartbeat (all-to-one)

* Complexity
  * Number of messages: $O(N)$
  * Network traffic: $O(N)$
  * Load: $O(1/T)$

* Algorithm
  * Every process periodically sends its state to main process

### Heartbeat (all-to-all)

* Complexity
  * Number of messages: $O(N^2)$
  * Network traffic: $O(N^2)$
  * Load: $O(N/T)$
* Algorithm
  * Every process periodically sends its state to all other processes

### No Timeout

* Algorithm
  * Every process stores list of neighbours and counters of received neighbours' state
  * Every process periodically sends its state to neighbours
  * On receiving other's state process updates counter and sends state to other neighbours
  * Failure detector shows counters without any interpretation

### Gossip-Style

* Complexity

  * Message size: $O(N)$

  * Number of messages: $O(N\log N)$
  * Network traffic: $O(N^2\log N)$ 
  * Load: $O(N\log N/T)$

* Algorithm

  * Every process stores membership list with heartbeat counter and last update time
  * Processes periodically gossip their membership list
  * On receiving other's membership list, the local membership list is updated
  * If no message by some process within time $T_{suspected}$ then this process is suspected
  * If process is suspected within time $T_{fail}$ then this process is removed from membership list

### SWIM

* Properties
  * Completeness
  * High accuracy (it growths exponentially with $K$)
  * Scalability
  * Time of failure detection doesn't depend on $N$
* Complexity
  * Number of messages (avg): $O(N)$
  * Network traffic (avg): $(N)$
  * Load (avg): $O(N/T)$
* Algorithm
  * Every process periodically ping other random process
  * If no response received from process $P$ within time $T_1$ then process ping other $K$ random processes asking to ping $P$
  * If still no response about process $P$ then $P$ is failed and removed from membership list

### $\varphi$ Accural Detector

* Algorithm uses statistics for determining $T_{fail}$

### FALCON

_Fast And Lethal Component Observation Network_

* Algorithm kills process that is counted as failed

### Panorama

* Algorithm uses all communication messages between processes for determining state of process
