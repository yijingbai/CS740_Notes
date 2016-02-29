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


