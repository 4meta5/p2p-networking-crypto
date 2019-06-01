# Libp2p Notes
> * [Rust implementation](https://github.com/libp2p/rust-libp2p) of [libp2p networking stack](https://libp2p.io)

* [Documentation for libp2p transport and protocol upgrade system](https://github.com/tomaka/libp2p-rs/blob/7aacb081d2e7db05b17c932370c926bb5e0d6230/libp2p-swarm/README.md)

*ToDo*
* [Experiment with `Cap'N Proto` for Libp2p](../basics/rpc.md)
* Play with Proxy Re-Encryption -- [Parity PRE Implementation](https://github.com/paritytech/xpremtinel)

## Why Libp2p
> [Why Libp2p](https://www.parity.io/why-libp2p/)

A fundamental shift in distributed computing is that the “client/server” paradigm no longer holds up. Let’s take a look at what your home router does. Every device in your home network has a private IP address. When you request data from a server, your router replaces your device’s private address with your home’s public IP address, and remembers which device to send the response to.

That works fine if all your devices are clients, but what about when a request from the outside world shows up at your router? It’s not a response to a request, it is a request, so the requestor thinks that you are a server. One of your devices is acting as a server, but your router doesn’t know which one. This is a problem called **NAT traversal**, and libp2p provides tools to help handle it.

## <a href = "https://www.youtube.com/watch?v=zcWHamr5m_k">The Life of a Libp2p Connection</a>
> Jacob Heun, Libp2p Developers Meeting July 2018

Basically you have the hash of a node's public key so you can use that to bootstrap a secure connection. Then you can enable privatization by basically adding an encrypted nonce that you send with each message.

*What are the benefits of privatization?*
* enables circuit routing through intermediaries
    * **why can't this just be done on the encrypted connection; what does circuit routing add in this case?**

> why are man-in-the-middle attacks even a thing? Key exchange algorithms and public key cryptography are invulnerable to them and we never communicate unencrypted so **WTF IS A MAN IN THE MIDDLE ATTACK IN TODAY'S CONTEXT?** (is it just a buzzword for people who aren't hip?)...**answer: trusted setups are vulnerable to man-in-the-middle attacks (hard coding node addresses for peer discovery)**

### Main Libp2p Protocols
> `libp2p/protocols/`

* `secio`, which is responsible for encrypting communications
    * `mplex` or `yamux`, which are protocols on top of `secio` that are responsible for multiplexing (`libp2p/muxers/`)
* `kademlia`
* `floodsub`
* `noise`
* `observed`
* `ping`
* `plaintext`