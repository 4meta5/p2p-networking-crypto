# Part I: Distributed Hash Tables

*This is Part I of a n-part series on Distributed Hash Tables*

Distributed Hash Tables (DHT) enable the coordination of thousands of computers to share resources without relying on a central registry. By storing resource locations throughout the network, DHT protocols are significantly more reliable and efficient than centralized databases and [flood-based approaches](https://en.wikipedia.org/wiki/Gnutella#Design) for node discovery and routing.

Contemporary p2p networks often extend or borrow heavily from the [Kademlia](https://en.wikipedia.org/wiki/Kademlia) DHT to coordinate peer discovery and establish order at the network level. With the advent of privacy-preserving DHTs, these networks *may* finally
1. patch [Eclipse](#eclipse) attack vectors
2. achieve decentralized peer discovery

*A few projects using DHTs at the network layer include [Ethereum](https://www.ethereum.org/), [IPFS](https://ipfs.io/), [dat](https://dat.foundation/), and [Polkadot](https://polkadot.network/) among others*

## Kademlia Primer <a name = "kademlia"></a>

DHTs coordinate the management of a [key-value store](https://en.wikipedia.org/wiki/Key-value_database) across a peer-to-peer network of nodes. Any participating node can *efficiently* retrieve the value associated with a given key. This structure strives to embody the following [properties](https://en.wikipedia.org/wiki/Distributed_hash_table#Properties):
1. autonomous and decentralized
2. fault tolerant
3. scalable

Kademlia uses [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol) for communication. Nodes coordinate an [overlay network](https://en.wikipedia.org/wiki/Overlay_network) in which each node is identified by a *node ID*. This network maintains resistance to denial-of-service attacks by routing around unavailable nodes `=>` **(2) fault tolerant**.

The *node ID* is used to locate values in the distributed key-value store. Specifically, this unique identifier provides a direct map to where values are stored (i.e. in which node(s) they reside). [To search for a value in Kademlia](https://youtu.be/w9UObz8o8lY?t=357), a participating node uses the associated key as well as the calculation of a *distance metric* between two nodes. The distance is computed as the [XOR](https://en.wikipedia.org/wiki/Exclusive_or) of the two node IDs. XOR was chosen because it acts as a sufficient distance function while also staying cheap and simple to calculate. Moreover, XOR ensures
* *identity*: the distance between a node and itself is 0
* *symmetricity*: the distances calculated from A to B and B to A are the same
* *[triangle inequality](https://en.wikipedia.org/wiki/Triangle_inequality)*

[Routing tables](https://en.wikipedia.org/wiki/Kademlia#Routing_tables) are constructed to facilitate the Kademlia search algorithm. Each search iteration achieves progress such that a basic Kademlia network with `2^{n}` nodes only takes `n` steps (in the worst case) to find the target node. Likewise, information lookups require contacting only `O(log(n))` nodes out of the total `n` nodes that comprise the network. This makes search across the network very efficient `=>` **(3) scalable**.

*Read more about Kademlia's algorithm for [locating nodes](https://en.wikipedia.org/wiki/Kademlia#Locating_nodes) and [resources](https://en.wikipedia.org/wiki/Kademlia#Locating_resources)*

## The Eclipse Attack <a name = "eclipse"></a>

An attentive reader might have noticed that we did NOT justify the first property in the [Kademlia Primer](#kademlia): **(1) autonomous and decentralized**. Although Kademlia generally prefers long-lived nodes to newer entrants, verification of a node's lifetime is not always straightforward. 

Kademlia requires each participating node to store a local routing table for the mapping of a subset of the network's nodes. Because the protocol does not require synchronization, there is no absolute truth for which node IDs are mapped to which address. 

Moreover, *every node defines its own ID*. Every time a node requests a connection, it sends its public key. The public key undergoes Keccak hashing before it becomes the node ID. 

With this in mind, an attacker can use one machine to create many node IDs, surround an existing node (or group of nodes), and launch a [Sybil attack](https://en.wikipedia.org/wiki/Sybil_attack). In doing so, the attacker confuses the surrounded node(s), feeding false data from the generated fake nodes. We refer to this as an **Eclipse attack**.

The basic problem arises as the result of the Kademlia peer discovery algorithm. An attacker can very easily craft a set of node IDs distinctly designed to fill all the buckets in a victim's peer mapping (by appearing to be *close*, according to the XOR distance metric). The deterministic and transparent nature of the distance metric makes it easy to isolate the victim from the rest of the network, manipulate in/outbounding connections, and control the victim's local view. While the paper linked below isolates a few workarounds to patch this attack vector, a better solution will likely involve decentralized peer discovery -- this is where privacy-preserving DHTs become relevant.

*Read more about eclipse attacks in [Low-Resource Eclipse Attacks on Ethereum's Peer-to-Peer Network](https://eprint.iacr.org/2018/236.pdf)*