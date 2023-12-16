### Main causes of tail latency?

* Workload bursts
* Interference with other processes, including background processes that run even on a system seemingly dedicated to a single server
* Request re-ordering caused by scheduling policies that are not designed with tail latency in mind
* Application-level design choices involving how transport connections are bound to processes or threads
* Multi-core issues such as how NIC interrupts and server processes are mapped to cores
* CPU power saving mechanisms

