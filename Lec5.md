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



