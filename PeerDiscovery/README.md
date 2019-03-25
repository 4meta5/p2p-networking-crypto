# Peer Discovery Research
> [Substrate's Network Layer](https://forum.parity.io/t/substrates-network-layer/174)

Currently, nodes join the peer-to-peer network using bootstrap nodes (addresses are hardcoded in the chainspec).
* vulnerable to man-in-the-middle attacks
* useful for development purposes

Each node holds a topology, containing the nodes that are known to be part of the network. The topology is cached on the disk, and is loaded on startup if it exists. Each node in the topology has a reputation ranging from 0 to 100. Keep in mind that a node’s reputation value is local, as each node posesses its own local topology.
* reputation is determined based on iterative Kademlia queries on random node IDs
    * more details in the forum post at the top of this doc

* [An introduction to Kademlia and How it Works](http://www.gleamly.com/article/introduction-kademlia-dht-how-it-works)

There are quite a few problems with using Kademlia:
* fate of the network is entrusted in very few (maybe 20) nodes
    * *The libp2p Kademlia system has a two additional messages, ADD_PROVIDER and GET_PROVIDER, which allows asking the peers closest to a key (which would be the hash of the name of the chain) to store the topology.*
* bootstrap nodes can connect to a limited number of other nodes
    * *The fact that a node accepts only up to a certain number of connections is problematic for bootstrap nodes. A freshly-created node with an empty topology needs to be able to connect to boostrap nodes, otherwise it will not be capable of joining the network. If all bootstrap nodes are full, a node can be denied entry.*
* **Kademlia discovery mechanism is vulnerable to Eclipse Attacks (similar to Ethereum)**
    * [Low-Resource Eclipse Attacks on Ethereum's Peer-to-Peer Network](https://www.cs.bu.edu/~goldbe/projects/eclipseEth.pdf)

Maybe two approaches to solving these problems (according to the aforementioned post in Parity forum)
1. establish strong, trusted, durable links between nodes
    * more easy to attack, but purportedly necessary for Polkadot to function
2. weak short-lived anonymous links based on randomness
    * [BRAHMS Discovery Mechanism: Byzantine Resilient Random Membership Sampling](http://www.cs.technion.ac.il/~shralex/Brahms-Comnet-Mar-15.pdf)

## Discovery Mechanism

The **Discovery Protocol** enables us to build a partial view of the network that we'll use for **[gossiping](./Gossip)**.

* [Rendezvous Protocol Research](https://github.com/libp2p/specs/blob/4059338ff0f90835ed953e1eb4c2beb703cc04d9/rendezvous/README.md)

## TODO (Substrate-related)
1. A discovery mechanism for the nodes of the global network.
2. A gossiping system for nodes to notify when they want to join or leave a specific network. This lets each node build a partial view of the chains it wants to be connected to.
3. An overlay-network-building gossiping system for each per-chain network.

The libp2p team right now is trying to implement [gossipsub](https://github.com/libp2p/specs/tree/master/pubsub/gossipsub), which combines the items 2 and 3. The rendezvous protocol 2 that they propose combines items 1 and 2.

Pierre thinks that we should replace the random Kademlia walking with a more simple system. I also think that we should improve Substrate’s reputation system to be closer to one of the reference papers about overlay networks.

## Other ReadingQ

* [SWIM](https://www.semanticscholar.org/paper/SWIM%3A-Scalable-Weakly-consistent-Infection-style-Das-Gupta/87123307869ac84fc16122043a4a313604bd948f)
    * [Smudge (go impl)](https://github.com/clockworksoul/smudge)

* [Habitat (Butterfly)](https://www.habitat.sh/docs/internals/#bootstrap-internals)
    * [Newscast Gossip](http://www.cs.unibo.it/bison/publications/ap2pc03.pdf)