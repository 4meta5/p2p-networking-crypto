# High Performance Browser Networking (notes)
> by Ilya Gregorik

## Speed as a Feature, Latency, and Bandwidth

**Speed is a feature**
* Faster sites lead to better user engagement.
* Faster sites lead to better user retention.
* Faster sites lead to better higher conversions.

**Latency**: the time from the source sending a packet to the destination receiving it <br>
* *Propagation Delay*: Amount of time required for a message to travel from the sender to receiver, which
is a function of distance over speed with which the signal propagates.
* *Transmission Delay*: Amount of time required to push all the packet’s bits into the link, which is a func‐
tion of the packet’s length and data rate of the link.
* *Processing Delay*: Amount of time required to process the packet header, check for bit-level errors,
and determine the packet’s destination.
* *Queuing Delay* Amount of time the incoming packet is waiting in the queue until it can be pro‐
cessed. <br>
Total latency is the sum of all of the delays listed above.

**Bandwidth**: maximum throughput of a logical or physical communication path


