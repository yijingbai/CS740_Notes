# Announcement: Midterm in ~2 weeks
# Last time: TCP Remy
# Today: Fast Updates
## Background
CK74->Specifics architecture for coummunacation between network foundation. = IP(narrow waist)

As networks evolved, the first method for assigning address space = classful addressing(classful = allocation on fixed/limited boundaries)

Solution to limts of classful allocation = supernetting via CIDR -> Allows allocations or powers of 2 uses "/" notation e.g. 128.22.18.1/14 or 45.0.0.0/8

BGP is used to tranmit network availability infernation throughout the Inetnet as #'s are required to facilitate BGP routes. => Repositories of BGP activity are available from Route views RIPE

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

