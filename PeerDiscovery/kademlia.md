# Kademlia

Kademlia defines a distributed hash table that stores data in the form of key-value pairs. 

Nodes in Kademlia are knowledgeable of other nodes -- this allows routing queries through low latency paths. Moreover, Kademlia leverages parallel and asynchronous queries which avoid timeout delays from failed nodes.

### Pros

* strives to minimize the number of inter-node introduction messages
* resistant to some DOS attacks

### Cons

Kademlia discovery mechanism is vulnerable to eclipse attacks

## Resources
* [An introduction to Kademlia and How it Works](http://www.gleamly.com/article/introduction-kademlia-dht-how-it-works)