# Announcement: Midterm#1 -> Friday Mar 3
# Last Time: TCP Vagas
# Today: TCP Remy
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



