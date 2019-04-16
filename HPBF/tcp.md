# Building Blocks of TCP

TCP provides an abstraction of a reliable network running over an unreliable channel by handling
* retransmission of lost data
* **in-order delivery** (ordered bytestream)
* congestion control and avoidance
* data integrity

TCP trades accuracy of delivery for speed of transmission.

## Three-Way Handshake

All TCP connections begin with a three-way handshake. The client or the server must agree on starting packet sequence numbers (picked randomly).
1. **SYN**: client picks random sequence number `x` and sends a SYN packets (also includes TCP flags and options)
2. **SYN ACK**: server increments `x` by one, picks own random sequence number `y`, appends its own set of flags and options, and dispatches the response
3. **ACK**: client increments `x` and `y` by one and completes the handshake by dispatching the last ACK packet in the handshake.

Once the three-way handshake is complete, the application data can begin to flow between the client and the server.

The delay imposed by three-way handshake makes new TCP connections expensive to create `=>` connection reuse is a critical optimization for any application running over TCP

**TCP Fast Open (TFO)** strives to reduce the latency penalty imposed on new TCP connections. The basic improvement is that TFO allows data transfer within the SYN packet. 