# Peer Discovery Research

## Substrate
> [Substrate's Network Layer (post by `tomaka`)](https://forum.parity.io/t/substrates-network-layer/174)

Currently, nodes join the peer-to-peer network using bootstrap nodes (addresses are hardcoded in the chainspec).
* vulnerable to man-in-the-middle attacks
* useful for development purposes (because of simple testing and setup)

Each node maintains a topology consistsing of the set of nodes that are known to be part of the network. Each topology is cached on the local disk and loaded upon startup. 

Every node tracks the reputation of its peers locally according to iterative Kademlia queries on random node IDs. See [current reputation algorithm here](./Gossip/rep.md).

> *[An introduction to Kademlia and How it Works](http://www.gleamly.com/article/introduction-kademlia-dht-how-it-works)*

There are a few problems with Kademlia in the context of Substrate:
* network control centralizes in very few (maybe 20) nodes
    * *The libp2p Kademlia system has a two additional messages, `ADD_PROVIDER` and `GET_PROVIDER`, which entail querying the peers closest to a key (in this case, the hash of the chain identifier) to store the topology.*
* bootstrap nodes can only connect to a limited number of other nodes
    * *A freshly-created node with an empty topology needs to be able to connect to boostrap nodes; otherwise it is incapable of joining the network. If all bootstrap nodes are full, such a node would be denied entry.*
* **Kademlia discovery mechanism is vulnerable to Eclipse Attacks (similar to Ethereum)**
    * [Low-Resource Eclipse Attacks on Ethereum's Peer-to-Peer Network](https://www.cs.bu.edu/~goldbe/projects/eclipseEth.pdf)

Possibly two approaches to solving these problems (according to the aforementioned post in Parity forum)
1. establish strong, trusted, durable links between nodes
    * more easy to attack, but purportedly necessary for Polkadot to function
2. weak short-lived anonymous links based on randomness
    * [BRAHMS Discovery Mechanism: Byzantine Resilient Random Membership Sampling](http://www.cs.technion.ac.il/~shralex/Brahms-Comnet-Mar-15.pdf)

### Discovery Mechanism

The **Discovery Protocol** enables us to build a partial view of the network that we'll use for **[gossiping](./Gossip)**.

* [Rendezvous Protocol Research](https://github.com/libp2p/specs/blob/4059338ff0f90835ed953e1eb4c2beb703cc04d9/rendezvous/README.md)

## TODO (Substrate-related)
1. A discovery mechanism for the nodes of the global network.
2. A gossiping system for nodes to notify when they want to join or leave a specific network. This lets each node build a partial view of the chains it wants to be connected to.
3. An overlay-network-building gossiping system for each per-chain network.

* [rendezvous](https://github.com/libp2p/specs/blob/4059338ff0f90835ed953e1eb4c2beb703cc04d9/rendezvous/README.md) seeks to solve (1) and (2)
* [gossipsub](https://github.com/libp2p/specs/tree/master/pubsub/gossipsub) seeks to solve (2) and (3)

## ReadingQ

* [SWIM](https://www.semanticscholar.org/paper/SWIM%3A-Scalable-Weakly-consistent-Infection-style-Das-Gupta/87123307869ac84fc16122043a4a313604bd948f)
    * [Smudge (go impl for SWIM)](https://github.com/clockworksoul/smudge)

* [Habitat (Butterfly Protocol)](https://www.habitat.sh/docs/internals/#bootstrap-internals)
    * [Newscast Gossip](http://www.cs.unibo.it/bison/publications/ap2pc03.pdf)

* [Big Brother Specs]()
    * attempt by Dean Eigenmann to bootstrap nodes in a decentralized manner by using the blockchain to coordinate peer discovery
    * [tomaka (pierre) tweet](): ~"*the base layer is still very centralized*"