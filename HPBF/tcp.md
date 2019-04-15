# Building Blocks of TCP

TCP provides an abstraction of a reliable network running over an unreliable channel by handling
* retransmission of lost data
* **in-order delivery** (ordered bytestream)
* congestion control and avoidance
* data integrity

TCP trades accuracy of delivery for speed of transmission.