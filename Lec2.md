######Lec2

architectures enable us to reason about complex systems

SRC84
Contribution: synthesized ideas that had been applied into a cogent definition.
Summary:

- Design principle for fun placement in a distributed system
- Cost/Benefit of pushing functions down to network
- Functions should only be pushed down to improve performance

Paper proceeds with example of simple/reliable data transfer
- One must be careful with using performance as the justification of pushing functions down because
performance could be the same if they implemented at higher levels.


Impact:
* Routers a fairly simple.
* TCP/IP are actually a part of the OS.
* Decentralized System.

Discussion
* Is TCP part of the end system?
* Consider bit level errors in packets - end or local?
* If QoS is required - where should it be implemented?
	- It is depend on the provider
	- The buffering is the simple example of QoS
* What's is primary benefit of an end-to-end implementation.
	- Reduces processing requirement in the network.
