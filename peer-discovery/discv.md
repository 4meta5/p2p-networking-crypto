# Discv
> *Discovery mechanism for node discovery on P2P networks*

## V4

* [devp2p/discv4](https://github.com/ethereum/devp2p/blob/master/discv4.md)

Kademlia-esque DHT that stores information about Ethereum nodes.
* Kademlia was chosen because it is an efficient way to organize a distributed index of nodes; also yields a topology of low diameter

## V5

* [devp2p/discv5](https://github.com/ethereum/devp2p/blob/master/discv5/discv5.md)

**Comparison with discv4**
* topic advertisement was added
* arbitrary node metadata can be stored/relayed
* node identity crypto is extensible, use of `secp256k1  keys isn't strictly required
* the protocol no longer relies on the system clock
* communication is encrypted, protecting topic searches and passive lookups against passive observers

* [Lighthouse Update #11 mentions an in-progress implementation for the rust-libp2p stack](https://lighthouse.sigmaprime.io/update-11.html)