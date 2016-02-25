# Announcements: Midterm#1 next Friday
# Last time: Algorithm for managing forward table
# Today: Tiny Terra
## Introduction

Paper presents a design for a small, high capacity router(320Gbit/s) at low cost that can serve as a foundation for next generation Internet routers.

## Basics for Routers

1. Switch packets
2. Participate in routing protocols

Input ports:
Lookup IP  | Header modifcation

Swtching Fabric

Output ports:
Buffers <= Buffer Management


--> Route Processor -->

switching fabric:

- shared memory
- shared bus
- X-bar - high performance
- networks - very high performance

Packaging: 

- Home routers: Very small, no cooling, low perf
- stackable: high port count, commodty processors
- Core/Backbone: High performance, cost, heat

power density 

## Packet processing switching 
route processing in Core Routers is done on line cards

- Easy: LongestPrefixMatching, header processing
- Hard: 
	- Buffering at very high rates > 10Gbit/s
	- Complex pkt processng
		- Solve performance realted problems through parallelism

## Switching Fabrics
It conneect input ports to output ports
Two basic desgn for routers based on capacty of swtiching fabric:

- input queued router -> Required when `capacity of inputs` > `capacity of switching fabric`. Drawbacks:HOL blocking
- output queued router -> Switching fabric have speed up capacity

## Tiny Terra
Input queued, Single Stage swtching core, with three major components:

1. nput ports
2. cross bar
3. output ports
Design goal is to support 32 * 10Gbps ports

Operations:

1. Packets arrive and are queued based on destination
2. scheduler decides cross bar config for each slots
3. Packet move to output queues to awat transmission

Scheduler and Crossbar are tightly integrated -> gathers info from input queues sets config for croess bar

Ports: Based on fixed size switching unit of 64-bits. Composed of serial links. Ports processor decides where to place data chunks in input or output queues.

Queues in tiny terra are managed via vitual outoput queueing(VOC) which eliminates HOL. It does this by defining a separate input queuee for each output -> required $N^2$ FIFO queues. In Order to make schduling decision on each "clock" cycle.

In order to make scheduling decision on each clock cycle $N^2$ queues must be considered. This is done via bipartite matching algorithm iSLIP.

Q: Why do we care about power density?
A: If we put more power into the chip, it could be hotter and hotter. May be we can cool this, we making the chips with higher and higher power density. But we could not keep cooling them. Reason: 

