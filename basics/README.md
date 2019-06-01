# High Performance Browser Networking (notes)
> by Ilya Gregorik

* [Speed as a Feature; Latency and Bandwidth](./latency-bandwidth.md)
* [TCP](./tcp.md)

## Highlights

**Latency**: the time from the source sending a packet to the destination receiving it <br>
* *Propagation Delay*: Amount of time required for a message to travel from the sender to receiver, which
is a function of distance over speed with which the signal propagates.
    * usually within a small constant factor of the speed of light
* *Transmission Delay*: Amount of time required to push all the packet’s bits into the link, which is a func‐
tion of the packet’s length and data rate of the link.
    * determined by the available data rate of the transmitting link
* *Processing Delay*: Amount of time required to process the packet header, check for bit-level errors,
and determine the packet’s destination.
    * once the packet arrives at the router, the router must examine the packet header to determine the outgoing route and check other properties for the data
* *Queuing Delay* Amount of time the incoming packet is waiting in the queue until it can be pro‐
cessed.
    * if the packets are arriving at a faster rate than the router is capable of processing, the packets are queued inside an incoming buffer

> *Total latency is the sum of all of the delays listed above.*