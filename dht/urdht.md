# Towards a Framework for DHT Distributed Computing
> https://scholarworks.gsu.edu/cs_diss/107/

UrDHT presents a unified model for DHTs. It attempts to extend DHT network topology to utilize Euclidean and Hyperbolic metric spaces.

## existing DHT topology

Routing typically works by minimizing the number of hops across the overlay network, with all hops treated as the same length. Indeed, DHTs typically are oblivious of the state of actual infrastructure the overlay is built upon...

**why?** Nodes are typically assigned a point in the range of a cryptographic hash function. The ID corresponds to the hash of some identifier `=>` this is done for load balancing and fault tolerance.

## future research

* embedding latency into the DHT while maintaining the system's fault tolerance `=>` discerning that the hops traversed to a destination are, in fact, the shortest path to the destination

> "We believe we can embed a latency graph in a hyperbolic space and define UrDHT such that it operates within this space. DHT with latency embedded into the overlay. Nodes respond to changes in latency and the network by rejoining the network at new positions." ~from paper linked above

## how

Each node is at the center of Voronoi region and responsible for the records that fall in that region. The peers the node connects with correspond to that node's Delaunay neighbors. 

> "*Because UrDHT works with the high-level abstractions of Voronoi Tessellation and Delaunay Triangulation, UrDHT can easily be configured to work in any arbitrary metric space or imititate the overlay topology of any DHT, such as Chord.*"

## references
* [UrDHT/PyUrDHT](https://github.com/UrDHT/PyUrDHT)