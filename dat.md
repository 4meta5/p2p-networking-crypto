# Dat Protocol

Dat is a protocol for sharing data between computers. 

## Discovery Mechanisms

Dat client uses several different methods for discovering peers from which they can download data. 

**Discovery keys** are used to find other peers interested in the same Dat. If a client has knowledge of a Dat's public key, the discovery key can be calculated. However, the discovery key cannot be used to discern the corresponding public key (*intuition behind public/private key generation via hash functions*). This prevents eavesdroppers learning of Dat URLs (and therefore being able to read their contents) by observing network traffic.

**NOTE**: eavesdroppers can confirm that peers are talking about a specific Dat and read all communications between those peers if they know the requisite public key; without the public key, eavesdroppers can still get an idea of how many Dats are popular on the network, their approximate sizes, which IP addresses are interested in them and potentially the IP address of the creator via observations of handshakes, traffic timing and volumes

To calculate the Dat's discovery key, `Blake2b` hash the public key (as 32 bytes, not 64 hexadecimal characters) concatenated with the word `hypercore`.

### Local Network Discovery

Local network discovery uses multicast DNS (like regular DNS query except instead of sending queries to a nameserver they are broadcast to the local network).

Switching to [Hyperswarm](https://pfrazee.hashbase.io/blog/hyperswarm) soon...

## Resources

* [How Dat Works](https://datprotocol.github.io/how-dat-works/)