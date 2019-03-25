## Gossip Protocols
> [wiki](https://en.wikipedia.org/wiki/Gossip_protocol); maintain a partial view of the peers of the network via some discovery mechanism

*Most gossip protocols work as follows (from node's perspective)*:
* To emit a message, transmit the message to all the nodes of a random subset of our partial view. 
* Upon receiving a message, transmit it to all the nodes of another subset of our partial view. 
* Gossiping the first message is random, but, thereafter, each node tracks which links were the quickest to propagate messages, to identify optimal connections and prioritize those connections in the future. 
    * **build a self-adjusting self-repairing tree on top of the partial view**

The term *convergently consistent* is sometimes used to describe protocols that achieve exponentially rapid spread of information. For this purpose, a protocol must propagate any new information to all nodes that will be affected by the information within time logarithmic in the size of the system (the *mixing time* must be logarithmic in system size).

## Protocols
* [Gossip-Based Broadcast](http://www.gsd.inesc-id.pt/~ler/reports/LPR_GossipBasedBroadcast.pdf) - Leitao, Pereira, Rodrigues
    * [Plumtree: Epidemic Broadcast Trees](http://www.gsd.inesc-id.pt/~ler/docencia/rcs1617/papers/srds07.pdf)
    * [HyParView](http://asc.di.fct.unl.pt/~jleitao/pdf/dsn07-leitao.pdf)