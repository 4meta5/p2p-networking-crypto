# High Performance Browser Networking (notes)
> by Ilya Gregorik

## Speed as a Feature; Latency and Bandwidth

**Speed is a feature**
* Faster sites lead to better user engagement.
* Faster sites lead to better user retention.
* Faster sites lead to better higher conversions.

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

*Total latency is the sum of all of the delays listed above.*

Cause -> Effect Relationships
* farther distance between source and destination +> INCREASED propagation delays
* more intedrmediate routers along the way => higher processing and transmission delays for each packet
* the higher the load of the traffic along the path => higher the likelihood of queueing delays

**Bandwidth**: maximum throughput of a logical or physical communication path

The ratio of the speed of light and the speed with which a packet travels in a material is known as the **refractive index**
of the material. The larger the value, the slower light travels in that medium.
    * The typical refractive index value of an optical fiber, through which most of our packets travel for long-distance hops, can vary between 1.4 to 1.6
    *  rule of thumb is to assume that the speed of light in fiber is around 200,000,000 meters per second, which corresponds to a refractive index of ~1.5

**Content Delivery Networks**: by distributing content around the world and serving that data from a nearby location to the client, we can significantly reduce the propagation time of all the data packets

**Last-Mile Latency**: 