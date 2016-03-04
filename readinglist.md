# Advanced Networking Reading List

\# |Date | Bib | Title | Type | Reviewed
---|-----|-----|-------|------ | -------
[Lec1](#toc_1)|Tuesday 1/26 | [SRC84](http://pages.cs.wisc.edu/~pb/740/end-to-end.pdf) | End-To-End Arguments in System Design | Internet Architecture adn Design
[Lec2](#toc_1)|Tuesday 1/26 | [CK74](http://pages.cs.wisc.edu/~pb/740/CK74.pdf) | A protocol for packet network ntercommunication | Internet Architecture adn Design
[Lec3](#toc_2) | Thursday 1/28 | [JK88](http://ee.lbl.gov/papers/congavoid.pdf) |  Congestion Avoidance and Control | Congestion Control
[Lec4](#toc_5) | Thursday 2/4 | [C](http://www.princeton.edu/~chiangm/milcom.pdf) | Layering As Optimization Decomposition:  Questions and Answers | Intenet Architecture and design
[Lec5](#toc_8) | Tuesday 2/9 | [LTWW94]() | On the Self-Similar Nature of Ethernet Traffic | Measurement and Modeling
[Lec6](#toc_22) | Thursday 2/11 | [ZDPS01]() | On the Constancy of Internet Path Properties | Measurement and Modeling
[Lec7](#toc_28) | Tuesday 2/16 | [BOP94](ftp://ftp.cs.arizona.edu/xkernel/Papers/vegas.ps) |  TCP-Vegas: new techniques for congestion detection and avoidance | Congestion Control
[Lec8](#toc_33) | Thursday 2/18 | [WB13](http://conferences.sigcomm.org/sigcomm/2013/papers/sigcomm/p123.pdf) | Winstein & Balakrishnan, "TCP ex Machina:  Computer-Generated Congestion Control" | Congestion Control
[Lec9](#toc_41) | Tuesday 2/23 | [SV00](http://cseweb.ucsd.edu/~varghese/PAPERS/sig2000final.pdf) |  Memory-efficient State Lookups with Fast Updates | HighSpeeding Routing | Reviewed
[Lec10](#toc_45) | Thursday 2/25 | [MIMEH97](http://arxiv.org/pdf/cs/9810006.pdf) | The Tiny Tera: A Packet Switch Core | High Speed Routing | Reviewed
[Lec11](#toc_51) | 3/1 | [ZDESZ93](http://pages.cs.wisc.edu/~pb/740/rsvp.pdf) | RSVP: A New Resource Reservation Protocol | QoS | Reviewed
[Lec12](#toc_) | 3/3 | [SSZ98](http://www.cs.berkeley.edu/~istoica/papers/csfq-sig98.pdf) |  Core-Stateless Fair Queueing: Achieving Approximately Fair Allocations in High Speed Networks | QoS | Reviewed

# Lec2

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


# Lecture 3 TCP/IP

## Next Paper: JK88
## Today: TCP/IP

OSI began to think about how to describe network function and functionality in late 70s.
First layered model in 83
Supercded by Internet Model. The key for the Internet Model: narrow waist
ie. one protocol at layer 3 = IP

Doyle + Csete "Arcitecture, Constraints and Behavior"

A protocol for packet Network Intercommunications

```
         ?
Net A <-----> Net B
```
Focus: How do we communicate across packet switched networks?

package switched network
Network Featrue: 
They could be different in :
1. Adresses
2. MTV
3. Error Handling
4. Timing
5. Status Information

Guiding principle for developing inter-network interfaces = simplicity
Economic component also plays a role.

Leave host communication to hosts -> in forms E-2-E arguments.

Gateway = hardware between networks that handle transfer of packets in a standard format -> Fig 3+2

The gateway is not router. For gateway, Net A and Net B using different technology.

Addressing must be standardized to facilitate wide area routing.

Packets that move between networks will be encapsulated with standard header.
Fragmentation may be required as a packet moves between networks.

Paper also mentions the need for capabilities to do network accounting.

Paper defines a Transmission Control Program(TCP).- Runs on hosts and handles the
transmission/reception of messages specifies.
That TCP must mux/demux between apps and the communication system.This is done
through the Process header which includes address and part $ TCP addressing.

TCP addressing: 8 bit for network, 16bits for TCP identifier

Details of the header are discussed: Segment Number, Flags, Checksum
                                          --> Support for fregmenting packets
                                          --> Must be aware of buffers that are used to manage flow control and reachability
                                    Reliability --> Facilited via `ACKs` and `timeouts`.

Solution for deal with duplicate and last packets includes the use of windows
implemented as queue on sender/receiver

Careful consideraton of TCP I/O -> there are significant implications for performance in terms of the implementation.

Final Part of the paper discusses the idea of "Connnections" --> Occurs

# Lec4 Congestion Avoidance and Control
## Announcements: 
Partners for projects
Project proposal due in 2 weeks.

Last time: CK74
Today: JK88

## Congestion Avoidance and Control

Motivation: Congestion had become a significant issue -> serous performance degradaton

    -----
--->    |
    ----
    buffer

How much buffering should be allacted in routers?
    -> Cost vs Perfermance benefit

More buffers:
    Costs more
    Lower probability for loss(??)
    Potential for lower delays()

Less buffers:
    Less cost
    more probability loss(?)
    lower the max delay of the network

Choosing: 
-> Empirical:We set up a experiment to measure them.
-> Models

In practice, buffers are provisioned by delay * bandwith
    --> See villamizar & Sorg 94

Summary: describes improvements in TCP that became known as TCP Tahoe.

Congestion is managed based on packet conservation principle.
-> Idea is that a system in equilibrium will only put a new packet into the system
when an old packet leaves the system.

Only three reasons why it fails:
1. The connection network doesn't get to equilibrium.
2. A sender injects a new packet before an old packet leave;
3. The equilibrium can't be reached because of resource limits a long the path.

These three reason form the basis for 7 improvements.
1. Getting to equlibrium via slow start. -> Solve the key issue of when to send packets
    -> prior to JK88, the only limit was RWND.
    -> Shown in Fig1

Conservation is restarted as "self clocking"
How is self-clocking bootstrapped?
Add a new variable to window control -> CWND
CWND = 1 when the transmission starts or after packet loss, CWND++ when an
ACK is received.
Set SWND = min(CWND, RWND)

This results in an expontial increase in packets for each RTT.
// TODO: Understanding this part.
2. Conservation equlibrium via improved RTO estimation prior to this work RTO est via EWMA.
JK state that timing est is the most important feature @ equlib Focus for timing estimates is on variation in RTT's

OLD: RTT = alpha\*EstRTT + (1-alpha)SRTT
     RTO = 2 \* EstRTT
    --> See Fig 5

Diff = SRTT - EstRTT
EstRTT = EstRTT + (d \* Diff) 0 < d < 1
Dev = Dev + d \* (|Diff| - Dev)
RTO = mu * EstRTT + phi \* Dev   mu=1 phi=4
    --> See Fig 6

JK also recognize that managing timing after packet loss is important. They adopt karn-Partidge
to manage timers in this case.

karn-Partidge Algorithm including two part：
1. We don't take sample RTT.
2. If we first lose, resend, double the time, and keep double
In a chaoic system .........

Adapt to path characteristics via Congestion Avoidance.
-> The key obvervation is that the signal for the congestion is that there is a lost packet.
i.e. a timeout. In this situation, SWND should be decreased exponentially. 
This is done by cutting send window in half.

After recovery, we probe for bandwith by increasing CWND by 1 each RTT.


# Lec5
## Announcements: 
Next Paper LTWW94
Midterm 3/8, 4/26
Project proposals due 2/12
   - Research questions
   - Related work
   - Technical Approach

**Midterm like Networking qualify questions**

Last time: Congestion Avoidance
Today: Optimization decommposition

## Congestion Avoidance
The Paper mentions Fast retransmit but doesn't go into detail.
Fast retransmt: three repeat ACK . Coupled with fast Recovery(the Congestion window just drop to half)
in TCP Reno.

Gateways have a perspective on congestion and thus have an opportunity to participate in congestion control.
Not really Congestion Avoidance control.

Last paper on architure: 

## Layering as optimization decomposition: Q & A
### Background

How can we improvement the architure. Improvement in TCP today, probably hard to do.
Still opportunities. Give ways to think about . Key objective: for improving the Internet
-> Performance. How do do we approach this?
-> E-2-E says -> push down if it improves performance.
More background: understanding impirical behaviour.  Measurement can provide insights that lead to Improvement.
Build a model. to provide insights.

Optimization is a way to understand system behavior.
Basic Idea: assume have function: $f(x)$ <- utility function. Find $x^*$ such that $f(x*) \geq f(x)$ for all $x$ or $f(x^*) \leq f(x)$ for all $x$. This can be globally or locally defined.

Stand methods for finding $x^*$ e.g. linear programming using simplex algorithm.

History: In mid 90's kelly considers network performance as a proposal fairness problem.
Low follows this with by defining **"optimal flow control"** Others made contributios to flow optimization culmurating in TCP FAST.

### Content
Optimization offers a way to consider network design.Focus is within layers, across Layers and by redefining layers. They suggest an approach for analyzing protocols as solutoins to global optimization i.e. network utility maximization(NUM).

#### Overview
1. Much of network esign is ad hoc.
2. architectures offer opportunities for improvements.
3. while layering is fundamental, we lack a theory to reason about layers.
4. One way forward is to consider layers as solutions to NUM.
5. NUM enables reasoning about fucntional placement.

##### Q1) Is this a just a "cross layer design"?
No.
2 new ideas:
	* protocol are solutions to NUM.
	* And that NUM problems are a way to formulate network design that forces consideratoin of e.g. end user.

##### Q2) How are utility funciton selected?
1. consider users
2. opertors

##### Q3) what NUM formulations are there?
They offer kelly as an example.This is an open question.

##### Q4) Isn't this all about dual decomposition?
No.

##### Q5) How do you know which decomposition to pick? (Great question)
Open question. We do not know.

##### Q6) Try not have non-convex function.

###### Q7) Fluid models.


# Lec6
## Announcements: 
project proposals due Fri
midterm #1 - possible reschedule
					 
Last time: Optimization decompossition: framework to analysis the data
Today: Measurement: Self Similarity

How might get data from system. 
The other side have the ability to analysis the data.
The paper today is one way to analysis the data.

On the self-similarity of ethernet traffic
----

## Key finding
Establishes self-similarity as an "invariant" property of LAN traffic. It is a big deal. Impirical measurement. Finding something invariant is important. The notion is finding a invariant is kind of great. Since things change everyday. It even more profound the internet grow the world of circuit swithching.
## Historical Background
Despite the fact that packet switching answer the basis for the Internet, telephone companies were the drivers of deployment of Internet infrastructure.
|=circuit switching=| => have a long history of relying on queueing theory. 
How AT&T make decisions about how many infrasuture to deploy.
We have enough wire to deal with large amount of one-to-one connection. 
Basis for queuing model for telephone network was that call arrivals follow poisson Process(exponentially distributed)

## Starting point
Two engineers at Bellcore L+W built systems that enabled packet traces to be captured. 
- Ethernet traffic
- Good precision - $100\mu$ resolutoin
- pket lenth, status, 60 B data
- Gathered 4 data sets over 3 years.

The paper goes into detail on pkt capture device deployment in Bellcore in order to make the case that traffic is not "speical" in some way i.e. the traffic is representative of LAN traffic that would be fourd anywhere.

Devices are very expensive. Grab the packet offline
 
 ____   link	_____
 |___| ------ |____|
        |________
 		  	    |___| -- Store

preprocesss(filter) the packet and store it first. Streaming algorithms on data that might 
Applying fiters to the wire. The filter is triggers in someway.

In the middle 20th centry, the idea is coined from mathmation great book : "factoral geomity of nature". the very small scale lke the large scale. print

mandelbrot --> taqqu --> Willinger look at the virtualizations
Why get things into matrix. We can you matrix math to analysis these stuff.

Time series of LAN traces reveal scaling properties -> these were bursty characteristics over multiple levels aggregation. => This is very different from poisson processes which smooh as they are aggregated.

The figure 4 of paper of all paper.

Self-Similare processes are a way to model processes with long range dependence(LRD) which means processes that have autocorrelation that persists over long time scales.

Paper presents basc framework for self simiarity and (R)

Self-Similarity can be described in terms of the Hurst Parameter H which is related to to the speed of the decay of autocorrelation of the time series $r(k)~k^\beta, H=1-\beta/2$ For a ss times series $w/LRD \quad  1/2 < H < 1$ => this is very different from poisson processes which smooth as they are aggregated.
Paper describes how to model SS processes. this is followed by a set of 4 tests that are used to demonstrate that the LAN traces do indeed exhibit SS.
1. variance time plots.
2. R/S plots
3. periodigrams
4. whittles's method
Analysis showed that 0.8 < H < 0.95 SS increased over time.

Why do we care about this?
- Implies that bursts can exceed even large buffers.
- Provides a foundation for accurate modeling of network traffic.
- Led to development of ON/OFF model that explains SS.

Heavy tail distribution, by having that charactric, we have SS. It study LAN traffic. What about WAN traffic?

For wide area network, the SS is indeed.
Further support.
Another paper, 

# Lec7
## Announcement
Project proposals due tomorrow, print and put in mail box

## Last time
LTWW94

## Today
Path constancy

## Notes on measurement
1) Passive. E.g. packet capture(through flow export e.g. netflow, SNMP security). Using special hardware like packet filter, also high speed packet need more precise timestamp. Senior provider turn on flow export, they want to see how their network behavies.2)Active Measurement: send packet or stream of packets into the network to measure response. Trace router: to get the path between the source and destiment.Strengths of passive measurement: 1. Not introduce extra packets. 2. Get real detail of packets 3.ability to capture the same packets to reproduce same phonoma. Weakness of passive measurement: 1. Hard to do in the general context 2. Need to get permission to the network 3. Need to filter the data and preprocess the flow to store the flow.Strengths of active measurement: 1.get what you want to get. 2.easy to do in the general contextWeakness of active measurement: 1. The data is less, may need extra information 

# Lec8
## Announcement Midterm #1 date TBD today
## Last time: Path constancy

The idea is measuring lose is still important. 
## Delay Constancy
RTT is considered - but spikes are removed. 
==> This leads to a focus on "The body" of the delay distribution. They don't complete ignore.

What will lead to a spike in delay?
==>

Result: Delay CFRs are shorter than loss CFRs(10-30min)

Interarrvals between spikes are well modeled by IID, thus poisson in nature

- delays are not mathematically steady
- Operational: 80% of CFR are > 20min - Not operationally constant
- Predictive: Fairly Good

## Throughput:
Math: Few Traces steady for all 5 hours.

-  For 60%-70% of the time, $CFRs \approx 2.5$ hours
Operational: Fairly steady over a period of hours when comparing $maxl/min \approx 3$

## Today: TCP Vegas
Reno version of TCP requires pkt loss as an indication of congestion. Vegas is a set of enhancements to TCP senders that attempt to avoid loss through Fine=grained mesurement of RTTs.

Paper Organization: 1. Evaluation enviroment 2. Enhancement 3. Evaluation

## Evaluation Environment
four ways to test protocols:

1. mathmatical modeling. There are two approaches: one is fluid model. 
Benefit: we can prove it without test.
2. Simulation:
	- Pros: Gives more realism vs models. 
	- Cons: No posibility to measure 
3. Emulation:
	* difference: realism 
		* simulation environment is complete self-contained. They 
		* emulation: you run the code that potentially run in real network.
4. Test bed
5. Live deployment

Emulator included a cool tracing capability that presented a number of important features of transoport protocols.

## Enhancement:

1. Improving timer
	- existing version of Reno used a 500ms timer => lead to ~1.1sec delay to resend pkts that were lost.
	- Improvment: Add time stamps to all pkts. They also specify that received ACKs should trigger an evaluation of whether or not to resend based on time out.
2. Congestion avoidence
	- based on observation of RTT's which should grow as queues fill before a pkt get lost. => leads to drop in throughput
	- Measuring throughput depends on measuring BaseRTT(min RTT)
	- Adjust the window:
		1. Send window is adjusted up by 1 if $diff < \alpha$
		2. congestion window is decreased by 1 if $diff > \beta$
		3. congestion window stays the same if $\alpha < diff < \beta$
3. Slow down slow start (**Very bad Idea**)
	- Reno's primary region of loss is on slow start.
	- Enhancement: double send rate every other RTT. Most packets in the Internet if very small. It will put a very bad effect on the tranmission of small files.


# Lec9
## Announcement: Midterm#1 -> Friday Mar 3
## Last Time: TCP Vagas
## Today: TCP Remy
TCP Ex Mchina
Internet now includes diverse technology e.g. data centers, wireless, dedicated long haul.

Prior congestion cotrol alogrithm were designed witha relatively static notion of networks.


Tranform more like
```
===============	               ______
									    __|     |__	
===============       or      __      ___
										   |_____|
```



Prior versions of TCP were "hand crafted" This paper describes TCP Remy, which is based on an optimazaton model and machine generatd Based on input from environment Models are focused on sending behavior. -->  Approach is described and then examined through extensive simulation study.

Good summary of prior work. The focus is when we send packet.

What can we change in order to produce a "new" TCP?
--> Signal of congestion
--> How to respond the signal of congestion
--> Timing and Timeout
--> Startup behavior

Approach for Remy -> create a model for congestion control. This is a distrbuted model with uncerntainty Framework: 

1. Assume Markovian Network behavior -> Key parameter: BW, Prop Delay, #senders (If look at narrow context, it is probably right, but in the broder context, it will be wrong)
2. Traffic = on/off
3. Objective function - based on Fairness such that all flaws get an equal allocation of the bottleneck BW in the best case.
	Based on $\alpha-fairness$ -> see related work
	
	Byte orianted protocol, congestion control. With packets that are being sent. 

Maximization the utility function leads to algorithm that determines when to send packets.

Global optimization is not possible. Thus approximations are used for snd state using EWMA to model ACKs, send and RTT ration. ==> These are the signals considered when making decision about congestion.

=>These are the signals considered when making decisions about congestion RemyCC creates a look up table to decide which action to take when an ACK is received.

Actions includes:

1. multiple to CWND
2. Increment to CWND
3. Delay between sending pkts.

The table is generated by conducting a set of simulations of senders over a set of candidate networks toward the goal of maximizing the utility function.

Evaluation via simulation that compares with other TCP's.

They generate 3 versions of RemyCC based o ndfferent valeus of  f and network specifications.



# Lec10
## Announcement: Midterm in ~2 weeks
## Last time: TCP Remy
## Today: Fast Updates
## Background
CK74->Specifics architecture for coummunacation between network foundation. = IP(narrow waist)

As networks evolved, the first method for assigning address space = classful addressing(classful = allocation on fixed/limited boundaries)

Solution to limits of classful allocation = supernetting via CIDR -> Allows allocations or powers of 2 uses "/" notation e.g. 128.22.18.1/14 or 45.0.0.0/8

BGP is used to tranmit network availability information throughout the Inetnet as #'s are required to facilitate BGP routes. => Repositories of BGP activity are available from Route views RIPE

Geoff Huston -> www.potarcoo.com
Recent update: to BGP/network
LandScape => 600k
Router are responsible for moving pkts toward destinations Divideino control(Routing) and data planes(forwarding)

Destination-based forwarding requires a lookup in forwarding table(establshed via control plane). Table = <Net Prefix, interface> Prefxes can vary in size are often overlap LPM is used to make forwarding decision.

How do we make this fast? 

1. Fast HW
2. Algorithms - Based on tree structure called Trie.

The "Trie" is the standard way to organize a lookup table.

## Motivation
LPM can be the bottleneck in router. Tries offer the opportunity to scale however DRAM s not sufficient so SRAM is the alternative.

The paper desribes a set of resouce mananage metholds that enables SRAM to be used effiecnecy - enables up to ~ 250 prefixs.

Specific objective: ~250K prefix table -- 32ns lookups

Empirical work on BGP activity indicated the need to handle frequent updates.

Chip Model:
Two process: Search + Update
Chip receve search or update key and return results
Update invoke memory allocator which manages fixed areas of physical memory.

IP lookups: LPM via multibit trie that is compressed provdes worst case memory guarantees. 125 bts/prefix is required for worst case, 50 bits for comon

To satisfy the objective of 2015K prefix in 16mb SRAM using this datastructure=> requres 85% efficency.
Frame-based allocator:
Key aspects:
	
	1. Compaction
	2. No garbage collection(Since garbage collection is cost)
	3. Large Search/compaction ratio
	4. Local compaction

LFC(Lazy Frame Compactions) is described Frame Merging => Based on notion of dividing words into frame and merging by tracking flawed frame i.e. that is less than 50% filled 

Merging within frames is also possible approach for ... this is also described

segment hole compaction
if LFC is used by itself, opportunities to merge adjactent frames can be missed
Segment = max sequence of contiguously allocated blocks

Experiments are focused on demonstrating benefits of LFC and SHC vs std method for allocation


# Lec11
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

# Lec11 RSVP
## Announcement: EXAM#1 this friday
## Last time: Tiny Terra
## Today: RSVP
### Quality of Service
Base IP service model assures nothing.
Basic IP servie perfectly for large number of applications. In particular, apps that are "elastic" which means that they tolerate delay, jitter(抖动，颤动) and lost. => implies that we can have many such applications activite at any point in time.

However, there are many apps that are o so tolerant i.e. "inelastic" e.g. media and control application(have real time requirement)
Consider service models in contextof utility to users v.s. network Bandwidth

At time of RSVP paper, most paper thought media apps would be facilitated ia multicast.

### How can we accommodate inelastic demand?
 
 - application layer buffering
 - Resouce reservation => only in a single admin domain
 
 Example of capabilities for resource reservations Diffserve Intserve, Queueing mechanisms. MPLS and RSVP like protocols

RSVP Describes protocol for reserving resourceson routers for inelastic applications

Paper begins by describing capabilities that are required of any architecture that support QoS.

1. FlowSpec = A language used to describe traffic which includes i. the characteristic of the flow and ii. the level of quality requried.
2. A routing protocol that support interaction between devices and requests for QoS
3. Resource Reservation = ability to set aside resources at all points along a path.
4. admission control = method for granng or denying reserved resources in order to assure specified QoS.
5. packet scheduling = method to decide which packet to transmit based on QoS

### Desgin Goals:

1. Support for hetrogenous receivers
2. Graceful treatment of multicast group membership.
3. we can treat the multicast address as a channel of the TV.
4. Allows users to specify app needs so that aggregate resource reservaton accurately reflect demand.
5. Enable users to easily swtch between data streams such that resource reservations are not over whelmed
6. Gracefull treatment of trouting changes
7. keep protocol overhead low.
8. Keep protocol design independent of the other 4 architectural compoment.

### Design Principles

1. Receiver-initiated reservation
2. Separating reservation from packet filtering reservations do not determine which packets get access - they only specify the resources themselves. Filters determine which packet get access. but don't affect the reservation.
3. Provide different reservation styles. This determines how resource requests will be aggregated styles are ether no fitlers, fixed filters or dynamic filters.
4. maintain "Soft-state" for resource reservations in the routers. This enables protocol to adjust configuratons based on user demand. 
   - Hard-state: configuraton paramter, the system turn off and turn on, the paramter will be there again.
   - Soft-state: when a rotuer restart or have changes, the state will be restablished by the protocol
   Path state is mantain via "Path message" which are transmitted from sources towards clients and include flow spec.
5. protocol overhead control - care taken in No of messages, message size and did some message merging
6. modularity - allows for use of different types of admission control flow spec packet filters etc.

## QoS in practice today

- Multiprotocol Label Switching(MPLS) - means for Traffic engineering that enable operators to specify a fixed route four some subset of traffic.
- QoS extension for OSPF(RFC3220)
- DiffService widely 

# Lec12
## Announcement: Midterm#1 Tomorrow (CS1325)
## Last Time: QoS/RSVP
## Core Stateless Fair Queueing
DiffServ (RFC2474)
Protocol for provdng QoS by controlling different traffic types. The main idea is to label traffic using DSCP codes in the IP ToS fields. Based on QoS assigned to a given packet, each router will use a variety of mechanisms delver request/assigned quality. 
Mechanism include:

- Classify
- Mark
- Shaping - done by managing queues
- policing - admission control / dropping

## Background
The Definition of the Core: deep whitin provider network that have large flow of data.

jitter - the time between the consecutive packets, if the time is stable, the jitter is low, if the time is changed, jitter is high.

Congestion control mechanisms work better when router have the capability to allocate the bandwidth fairly.  -> This can be addressed by employing methods to manage queues in routers. The cost is increased complexty(i.e. requirement to maintain per flow state) CSFQ approximates FQ at greatly reduced complexity.

Fair bandwidth allocation: Does TCP guarantee Fairness

Assumptions: 

1. The Fair allocation of bandwidth is important to the congestion control.
2. The complexity of Fair Bandwidth allocation makes it prohibitive to deploy
3. FQ says that when demand > C flows are allocated min(f,r) where r =rate and f = proportional rate

CSFQ approximates FQ without maintaning per flow control in the core.

Assumes Edges/Core network config: 

1. core node is complete surrounded by the edge nodes.
2. Core node do not maintain per flow state
3. Edge nodes do maintain per flow stat and apply labels to packets that indicate rates.

CSFQ assumes a flow X has a rate $r_x$ it has a fair rate $f_x$ for a given router. if $r_x > f_x$ the next packet for the flow will be dropped(on input) with same probability $p=1-\frac{f_x}{r_x}$ To calculate $f_x$ at a given router, use an iteravive procedure for estimating the aggregate arrival rate A and an aggreagate accept rate F Then calcuate $f_x \approx r for A <= C$ and $fold * \frac{C}{F} for A>C$ For edge nodes, use exponential average to estimate $r_x$(reduces sensitivity to burstiness) - see formula #3

This method can be include weighting such that given flows get proportionally more/less bandwidth
This paper present a theorem that bounds the amount of extra BW a flow care receive.
Paper proceeds to evaluate CSFQ vs a variety of AQM methods e.g. FIFO, RED, FlowRED, DRR.
Edge routers n CSFQ have a complexity $\approx$ FRED which maintains state on a per flow basis.
Core router have complexity $\approx$ RED. Simulation config = single congested link, 1 UDP flow, 31TCP Flows, latency(1ms)

Result
1. CSFQ performs very close to DRR in terms of fair Bandwidth allocation and Throughput.
2. Consider multiple congested links and a single TCP flow in the face of a large amount of UDP traffic. Result: For UDP source CSFQ $\approx$ FRED and for TCP source CSFQ does much better than all other mechanisms except DRR.
3. condiders performance of CSFQ when other schemes are used at the same time.
4. Instances have on-off souce along wth 19 TCP sources and a single bottleneck.
Result: CSFQ is much better than FIFO or RED but worse than FRED.

Internet arch / TCP / Measurement / Router Design / QoS

Two question on each area
10 questons

1 hour 15 mins

