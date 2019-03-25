## Gossip Protocols
> [wiki](https://en.wikipedia.org/wiki/Gossip_protocol); maintain a partial view of the peers of the network via some discovery mechanism

* Then, when we want to emit a message, we transmit this message to all the nodes of this view or a random subset of this view. When we receive a message, we transmit it to all the nodes of another subset of our local view. 
* Gossiping the first message is random, but then each node keeps track of which links were the quickest to propagate messages, and connections that seems to work well are favoured. 
    * In other words, we try to build a self-adjusting self-repairing tree on top of the partial view.

The term *convergently consistent* is sometimes used to describe protocols that achieve exponentially rapid spread of information. For this purpose, a protocol must propagate any new information to all nodes that will be affected by the information within time logarithmic in the size of the system (the "mixing time" must be logarithmic in system size).

## Protocols
* [Gossip-Based Broadcast](http://www.gsd.inesc-id.pt/~ler/reports/LPR_GossipBasedBroadcast.pdf) - Leitao, Pereira, Rodrigues
    * [Plumtree: Epidemic Broadcast Trees](http://www.gsd.inesc-id.pt/~ler/docencia/rcs1617/papers/srds07.pdf)
    * [HyParView](http://asc.di.fct.unl.pt/~jleitao/pdf/dsn07-leitao.pdf)