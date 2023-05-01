# Implementations Showcase: Nabu - Java IPFS - Ian Preston

<https://youtube.com/watch?v=TzCxmxmW4jQ>

![image for Implementations Showcase: Nabu - Java IPFS - Ian Preston](/thing23/TzCxmxmW4jQ.jpg)

## Overview

In this video, Ian Preston from Pegos introduces Nabu, a Java IPFS implementation, and discusses the advantages of using Java for this application. He also shares the current status of the project and some real-world benchmarks.

## Why Java?

- Super popular language
- Universal (runs on mobiles, browsers, desktops, IoT, etc.)
- Strong corporate buy-in
- Faster than Go
- Durable and backwards compatible
- Runtime code loading

## What is Nabu?

Nabu is a minimal, embeddable IPFS implementation that lets users store and serve their data over standard libp2p. Its motto is "route your peers, not your content." Nabu allows users to direct requests to specific nodes, rather than simply broadcasting requests.

## Current Status of Nabu

Although only two months in development, Nabu is already usable with the following features:

- TLS security transport
- Early Mux negotiation
- Cadamly, including IPNS
- BitSwap with auth extension
- Secure API
- Peer-to-peer HTTP proxy
- Standard port mapping
- Bloom and InfiniFilter block stores

## Real-World Benchmarks

Nabu was tested with Pyrgos to mirror a DAG as quickly as possible:

- Tested DAG: 6 layers deep, ~6,000 blocks, ~20 MB
- Time taken with Kubo: 120 seconds
- Time taken with Nabu: 12 seconds

Based on these results, the BitSwap protocol performs well with sensible data structures.

## Conclusion

Nabu is a promising Java IPFS implementation with several useful features already in place. Feel free to try it out and give feedback. Finally, Ian thanks the IPFS fund for their support in this project.