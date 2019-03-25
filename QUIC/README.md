# QUIC Protocol
> protocol by Google sitting between `UDP` and `HTTP` which seems to minimize stream congestion (in the context of limited bandwidth) by decreasing handshake latency and invoking session tickets to instantaneously initiate encrypted connections thereafter.

* [QUINN](https://github.com/djc/quinn) -- futures-based QUIC implementation by `djc` and `Ralith`
* [Quiche](https://github.com/cloudflare/quiche) -- QUIC transport protocol and HTTP/3 by `cloudflare`

## <a href = "https://www.youtube.com/watch?v=4FvMed5iCb4">Marten Seeman's Talk at Libp2p 2019</a>

Normal connections run on the IP layer, with TCP on top (which manages packet loss, congestion control), with TLS as the encryption layer and HTTP/2 on top.

QUIC runs on top of **[UDP](./udp.md)** instead of TCP (UDP is on top of IP, and UDP is just packets). QUIC does all the crypto (everything but the key handshake), manages packet loss, and even handles some thing at the application layer.

### Problems with TCP
* connection establishment takes 3 network round trips
    1. client -> sim packet -> server
    2. server -> ack packet -> client
    3. client -> ack packet, ciphersuite options -> server
    4. server -> picks ciphersuite -> client
    5. client -> sends keyshare -> server
    6. server -> sends keyshare -> client
    * client starts data over the connection
**Why doesn't the server send their keyshare with the ciphersuite choice on step (4)? I realize the client still has to send its keyshare but, in this case, it can send its keyshare with the initial data.**

HTTP/1 suffers from head-of-line blocking because we have to wait for each item to be received before requesting subsequent resources. We could open up more connections, but we'll just suffer from difficulty with congestion control.

> Every connection initially sends a small amount of data on the network and then ramping up the amount of data it sends exponentially until a packet loss occurs (fight over bandwidth between connections under this paradigm is not optimal)

SPEEDY uses a single TCP connection. On top of that we add streams; stream multiplexing adds framing in order to send requests over multiple streams simultaneously on a single connection. The problem is that TCP is an ordered byte stream so it does not know how to deal with packet loss (in essence, TCP wasn't designed for streams). 

*We need a Transport that understands what streams are and is intelligent enough to deliver data out of order.*

TCP also suffers from ossification. In every TCP header, we store acks. Middleboxes *optimize* traffic by rewriting values in the TCP header. It might have worked fine under some benchmark, but this middlebox action prevents us from updating the protocol because things break in unpredictable ways. Even if this only effects 0.5% of the internet, you can't do that. Even if we could update it over a decade timespan, this is too slow.

> When Google invented QUIC, TLS 1.3 wasn't out. Now that TLS 1.3 is out, Google has replaced its previous crypto library (Google QUIC crypto) with TLS 1.3. 

### QUIC
In QUIC, we assume the client sent the right address with the first packet. Why? Because the first packet for QUIC requires a certain size (like 1200 kb). This massive first packet sent to the server is the client "hello" of the TLS handshake. If the server thinks its under attack, it can do something like the 3-way handshake (with a sim cookie verification protocol in which the server sends the client a sim cookie and the client sends back an authenticated message). 

The client also optimistically sends keyshares. It says "I support ciphersuites A, B, and C" and here are some of my keyshares for these.

The server sends the client a session ticket such that if the client ever wants initiate an encrypted connection again, it can just use the session ticket and send data on the first flight (zero round handshake).

On a TCP connection, you do the handshake and then the application provides data to TLS, TLS encrypts the data, puts it into TLS records and sends the TLS records over the wire. The TLS records have to be read in order `=>` head of line blocking on the crypto layer. 

For QUIC, the session key is exported to the QUIC layer as soon as the TLS handshake completes. The QUIC layer encrypts/decrypts data. Data is encrypted on the packet level -- we use an AEAD (Authenticated Encryption on Associated Data).

Every QUIC packet contains a header (which is unencrypted) and the data. The data is fed into the AEAD such that it also verifies the header to check if the ciphertext has been tampered with and you can see if the unencrypted header is really what was sent (because this is part of the encrypted payload). This means that middleboxes can read the header (which is unencrypted) but it can't modify it or else the connection will break (because the verification via the payload will fail). **This is designed to prevent the ossification of QUIC and enable future upgradability**

The QUIC packet is sent as the payload of a UDP packet. It starts with its own header. There are two types of headers -- (1) long headers are used during the handshake; (2) after the handshake completes, the short header is used which is designed to be small.

Long header starts with a type byte (identifying it as a long header) and a version header (4 byte number); when the server receives a packet from the client, it will see if it supports the version...if it doesn't support the version, the server will send back the versions it supports and the client will respond accordingly (either with one that the server supports or drop the connection).

> designed for upgradability -- now on QUIC version 44+

After the version fields, there are a few fields that contain connection IDs (destination connection ID, source connection ID, Connection ID length).

In TCP, you identify a connection by its IP address and its port (works fine in early internet days). Now, there are so many devices on so many wifis and networks; switching between IP addresses results in constantly losing the TCP connection.

With QUIC, connection is identified by connection ID so you can switch between interfaces (IP addresses) and maintain the same connection.

We can run all connections on the same port because we don't need the port to identify a connection. We can have a QUIC listener on a port and then we can dial out on the same port and demultiplex by looking at connection IDs.

In the short header, we have a type tag, destination connection ID, and the payload (optimized to send lots of data with minimal overhead). Payload of a QUIC packet is composed of Frames. STREAM data is carried in STREAM frames and ACK data (acknowledgements for received packets) is carried in ACK frames. When we compose a QUIC packet, we serialize the Frames one after another.

There are unidirectional streams (one side can open and can send and the other side can only receive). There are bidirectional streams in which both peers can send and receive data.

Streams are always accepted in the same order that they were created by the peer (even if packets are lost). This is very useful for building protocols on top.

You can open as many streams as you want. Over the lifetime of a connection, you can use as many streams as you want (it's a 62-bit space so you never run out of streams).

**Why isn't there head of line blocking for multiple streams over the same connection? [original question](https://youtu.be/4FvMed5iCb4?t=1517)**
* He basically claims its because you know the order in which the streams were sent so you know that the earlier stream should have arrived so you basically acknowledge that it is missing for the package loss (recovery protocol) and proceed to read the next stream. But **how do you know the order of the streams?**

**Loss Recovery**
* if you retransmit a packet in TCP, it gets the same packet number as the packet that was lost; then you don't know if the packet eventually received was the original packet or the one that was sent later. 
* QUIC uses a different packet number for each packet sent (even for loss recovery). (each packet has a monotonically increasing packet numbers)
    * no ambiquity with packet resubmission
* rick ACK frames
    * can encode hundreds of ranges (to tell us which packets were lost and how to recover)
* QUIC implementations live in user space (easier to update than updating the kernel every time)
    * RENO congestion control
    * Chrome uses CUBIC
    * future BBR
    * lots of experimentation with congestion control algorithms because experimentation is so easy

Libp2p Integration
* QUIC streams are like Libp2p streams (for bidirectional streams; no unidirectional streams because they aren't used in Libp2p)
* Secio doesn't work for QUIC handshake because it doesn't integrate with how QUIC works `=>` need the TLS handshake (designed protocol: create self signed certificate with the private key => sign an intermediate chain using that CA and put that as a certificate chain in the TLS handshake)
    * at the end of the handshake, you know the certificate chain of your peer
    * ensures you connect to the peer that you want to connect to

* [quicwg.org](https://quicwg.org)