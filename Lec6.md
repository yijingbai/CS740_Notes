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

