# How to Build Your Own Compatible libp2p Stack from Scratch in an Afternoon - Marten Seemann

<https://youtube.com/watch?v=aDHymXQJ4bs>

![image for How to build your own compatible libp2p stack from scratch in an afternoon - Marten Seemann](/thing23/aDHymXQJ4bs.jpg)

## Overview

In this video, Marten Seemann presents a talk about building a compatible libp2p stack from scratch in an afternoon. The presentation covers the selection of a QUIC stack, adding libp2p TLS extensions, implementing peer ID encoding, and sending multi-stream headers in QUIC.

## Article

Hello, I'm Marten Seemann, and today, we're going to discuss how to build your own compatible libp2p stack from scratch in an afternoon. Alongside Marco, we will explore what libp2p is and how it can benefit various applications.

### Libp2p Building Blocks

Libp2p comes with several building blocks that make it easier to build peer-to-peer applications. These building blocks include transports, multiplexers, secure channels, and peer discovery. In addition, libp2p has features such as NAT traversal, DHT, and GossipSub built on top of it, making it a popular choice for developers.

### A Modern Libp2p Stack

A modern libp2p stack consists of the following layers:

1. **Transport Layer**: Primarily QUIC, with fallbacks for TCP, WebSocket, WebRTC, and WebTransport.
2. **NAT Traversal**: Handles getting through NATs effectively, without having the developer worry about the complexity.
3. **Applications**: Building blocks like Cademlia and GossipSub, as well as any custom protocols for specific peer-to-peer tasks.

### Building an Interoperable Libp2p Client

To create an interoperable libp2p client, you need to have the following components:

1. **QUIC Stack**: Choose any QUIC stack, such as MSQUIC.
2. **Libp2p TLS Extension**: Add the libp2p TLS extension to your chosen QUIC stack.
3. **Peer ID Encoding**: Implement peer ID encoding, which is the `/P2P` stuff.
4. **Multi-stream Header**: Send the multi-stream header when negotiating streams in QUIC or other protocols.

### ZigLibp2p: A Lean Libp2p Stack

ZigLibp2p is a lean libp2p stack built using MSQUIC, Microsoft's C library for QUIC. This implementation has few dependencies, focusing on performance from day one. ZigLibp2p serves as a proof of concept that, starting from QUIC, it's possible to build a lean, interoperable node.

### Why Choose ZigLibp2p?

ZigLibp2p offers various advantages, such as:

- A proof of concept that shows how close you can get to libp2p using just QUIC.

- A C API that allows other implementations to bootstrap even quicker.

- An experiment with MSQUIC as a libp2p node, which is known for its speed and well-designed API.

- Offering a different libp2p library with minimal dependencies, focusing on performance, and making few assumptions about the caller's needs.

## Key Takeaways

- Libp2p is a popular choice for building peer-to-peer applications because of its building blocks, such as transports, multiplexers, and secure channels, and built-in features like NAT traversal, DHT, and GossipSub.
- A modern libp2p stack primarily uses QUIC at the transport layer, with fallbacks for other protocols.
- Developers can build an interoperable libp2p client using a QUIC stack, libp2p TLS extension, peer ID encoding, and multi-stream header.
- ZigLibp2p offers a lean libp2p stack that demonstrates how easy it can be to build an interoperable node using QUIC.
- Building on ZigLibp2p offers distinct advantages, such as its C API, its use of MSQUIC, and its focus on performance.