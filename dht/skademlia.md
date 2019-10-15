# S/Kademlia: A practicable approach towards secure key-based routing
> *[paper](https://www.researchgate.net/publication/4319659_SKademlia_A_practicable_approach_towards_secure_key-based_routing)*

Common service is the *key-based routing layer* (KBR). This layer provides efficient routing to identifiers called *keys* from a large *identifier space*. Every participating node in the overlay chooses a unique *nodeId* from the same id space and maintains a routing table with `nodeId`s and IP addresses of neighbors in the overlay topology.

Depending on the overlay protocol, the topology resembles a ring, hypercube, or de Bruijn graph. Every node is responsible for a particular range of the identifier space, usually for all keys close to its *nodeId* in the id space.

> KBR is used to efficiently route a message to an arbitrary key by successively forwarding the message to overlay neighbors which have a *nodeId* closer to the destination key.

## motivation

prevents sybil attacks on the address space by creating a minimum work threshold for node generation (storage NodeId generation requires *trailing* bits of 0s `=>` slows down process of adding new nodes)

## objects and behavior

### Secure `nodeId` assignment (section 4.1)

#### generate `nodeId`

hash over a public key

#### message signatures

To sign messages exchanged by nodes, differentiate between two signature types:
1. `weak_signature`: does not sign the whole message (limited to IP address, port, and a timestamp) `=>` use timestamp to determine how long the signature is valued (to prevent replay attacks); used for `FIND_NODE` and `PING` messages where the integrity of the whole message is dispensable
2. `strong_signature`: signs the full content of a message (to ensure integrity and resilience againt Man-in-Middle attacks); replay attacks prevented with nonces inside RPC messages.

Use `crypto_puzzle_sig` to impede Eclipse and Sybil attacks
* `static_puzzle` impedes how the *nodeId* can be chosen freely
* `dynamic_puzzle` ensures that it is complex to generate a huge amount of *nodeId*s

### reliable sibling broadcast

Common security problem is the reliability of sibling information which arises when replicated information needs to be stored in the DHT which uses a majority decision to compensate for adversarial nodes.
> see [10](http://www.cs.kent.edu/~javed/class-IAD06S/papers-2004/gai.pdf) for definition of a *sibling* list to manage certain lists of `(key, value)` pairs

The **routing table** consists of a list of `N` k-buckets holding nodes with a distance `d` with `2^{i - 1} \leq d \less 2^i, 0 \leq i \leq n` and a sorted list of siblings of size `n_sigma * s`

#### routing table maintenance

Categorize signalling messages to the following classes:
* incoming signed RPC requests
* responses
* unsigned messages

Each message contains the **sender address**. The sender address is *valid* if the message is signed and *actively valid*...

Actively valid sender addresses are immediately added to their corresponding bucket, when it is not full. Valid sender addresses are only added to a bucket if the *nodeId* prefix differs in an appropriate amount of bits `\psi` (`\psi > 32`).
> sender addresses that come from unsigned messages will be ignored

### lookup over disjoint paths 

use `d` disjoint paths to increase the lookup success ratio in a network with adversarial nodes; by using the sibling list, the lookup doesn't converge at a single node but terminates on *d* close-by neighbors, which all know the complete *s* siblings for the destination keys `=>` lookup is still successful even if `k-1` of the neighbors are adversarial