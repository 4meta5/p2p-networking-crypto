# Remote Procedure Call Stuff

*ToDo*
* use [Cap'N Proto Rust](https://github.com/capnproto/capnproto-rust) for Libp2p (in lieu of using protobufs)
    * [introduction](https://capnproto.org/)
    * [tutorial](https://abronan.com/getting-started-with-capnproto-rpc-for-rust/)

* [Parity JSONRPC](https://github.com/paritytech/jsonrpc)
    * `JSON-RPC`: A standard to call functions on a remote system using a JSON protocol. For Substrate, this is implemented through the Parity JSONRPC crate.
    * `JSON-RPC Core Crate`: Allows creation of JSON-RPC server handler, with supported methods registered. Exposes the Substrate Core via different types of Transport Protocols (i.e. WS, HTTP, TCP, IPC)
    * `JSON-RPC Macros Crate`: Allows simplifying in code the creation process of the JSON-RPC server through creating a Rust Trait that is annotated with RPC method names, so you can just implement the methods names
    * `JSON-RPC Proxy Crate`: Expose a simple server such as TCP from binaries, with another binary in front to serve as a proxy that will expose all other Transport Protocols, and process incoming RPC calls before reaching an upstream server, making it possible to implement Caching Middleware (saves having to go all the way to the node), Permissioning Middleware, load balancing between node instances, or moving account management to the proxy which processes the signed transaction. This provides an alternative to embedding the whole JSON-RPC in each project and all the configuration options for each server.
    * `JSON-RPC PubSub Crate`: Custom (though conventional) extension that is useful for Dapp developers. It allows "subscriptions" so that the server sends notifications to the client automatically (instead of having to call and poll a remote procedure manually all the time) but only with Transport Protocols that support persistent connection between client and server (i.e. WS, TCP, IPC)