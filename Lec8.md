# Announcement Midterm #1 date TBD today
### Last time: Path constancy

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

# Today: TCP Vegas
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


