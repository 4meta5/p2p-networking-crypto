# Token Bucket Algorithm
> similar to flow-control mechanism used for [Parity Light Protocol](https://github.com/ethereum/devp2p/blob/master/caps/pip.md)

The **token bucket** is an algorithm used in packet switched computer networks to verify that packets conform to defined limits on bandwidth and burstiness (a measure of the unevenness or variations of the traffic flow). 

The token bucket algorithm is based on an analogy of a fixed capacity bucket into which tokens (bytes or a single packet of predetermined size) are added at a fixed rate. 

A conforming flow can therefore contain traffic with an average rate up to the rate at which tokens are added to the bucket (and burstiness is determined by the depth of the bucket).

## Algorithm

1. A token is added to the bucket every `1/r` seconds.
2. The bucket can hold at most `b` tokens. If a token arrives when the bucket is full, it is discarded.
3. When a packet (network layer Protocol Data Unit) of `n` bytes arrives,
    * if at least `n` tokens are in the bucket, `n` tokens are removed from the bucket, and the packet is sent to the network.
    * if fewer than `n` tokens are available, no tokens are removed from the bucket, and the packet is considered to be *non-conformant*

## Use Cases

Commonly used in either traffic shaping or traffic policing. For traffic policing, noncomforming packets may be discarded (dropped) or may be reduced in priority. In traffic shaping, packets are delayed until they conform.

Traffic policing and traffic shaping are used to protect the network against excess or excessively bursty traffic.