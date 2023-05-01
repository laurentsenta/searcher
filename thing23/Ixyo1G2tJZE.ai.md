# The Incredible Benefits of libp2p + HTTP - Marten Seemann & Marco Munizaga

<https://youtube.com/watch?v=Ixyo1G2tJZE>

![image for The Incredible Benefits of libp2p + HTTP - Marten Seemann & Marco Munizaga](/thing23/Ixyo1G2tJZE.jpg)

## Overview

In this video, Marten Seemann and Marco Munizaga discuss the advantages of combining libp2p and HTTP to take advantage of the existing internet infrastructure and caching to create more efficient, secure, and universally supported communication. They also demonstrate a demo of how they successfully tested the integration of the two technologies.

## Content

Why do people use HTTP? For about 30 years, HTTP has been around and is universally supported by all browsers, command-line tools, and most programming languages. It is also backed by a robust infrastructure like CDNs, which makes distributing web content across the globe a fast and smooth experience.

LibP2P, however, does not enjoy the same universal support, but it comes with features that HTTP doesn't have, like NAT traversal and not relying on TLS certificates issued by certificate authorities. This led to the discussion within the LibP2P team about combining the benefits of both HTTP and LibP2P to make use of the amazing caching infrastructure and net traversal that LibP2P provides.

The goal is to enable developers to write an application that would normally run on an HTTP server and repurpose the same logic to run on the LibP2P stack. This would create better-connected and easily accessible decentralized networks like IPFS and Filecoin.

The integration of LibP2P and HTTP is possible because of the work the LibP2P team has done over the past year, implementing support for web transport and WebRTC, allowing browsers to connect to LibP2P nodes without needing TLS certificates. In the demo, Marco shows how they used a service worker to intercept HTTPS requests and serve the requests using LibP2P connections.

## Key Takeaways

- Combining HTTP and LibP2P can enable us to take advantage of existing internet infrastructure and caching to create more efficient, secure, and universally supported communication.
- LibP2P team added support for web transport and WebRTC, allowing browsers to connect to LibP2P nodes without requiring TLS certificates.
- Service workers can be used to intercept HTTPS requests and serve them using LibP2P connections, bypassing the TLS certificate constraints.