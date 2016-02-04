Announcements: Partners for projects
               Project proposal due in 2 weeks.

Last time: CK74
Today: JK88

Congestion Avoidance and Control

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


