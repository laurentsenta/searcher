# Live CDN Incentives and its Future - laudiacay

<https://youtube.com/watch?v=yrrAjR03TsU>

![image for Live CDN Incentives and its Future - laudiacay](/thing23/yrrAjR03TsU.jpg)

## Overview

In this video, Claudia, founder of Banyan, presents her research project that aims to offer decentralized retrieval of data at Content Delivery Network (CDN) speeds without relying on trust. Combining prior work on verified streaming and payment channels, she discusses potential improvements to existing systems, explores several possible solutions, and suggests some areas for future research.

## Content

Hello everyone, my name is Claudia, and I'm the founder of Banyan. Our company focuses on bringing data onto Filecoin. On the side, I also do research projects like this one on improving decentralized CDN incentives. 

Current solutions, such as retrieval pinning and Skynet's in-band incentives, have limitations when it comes to CDN speed and scalability. I propose a solution that combines elements from these existing systems while adding new improvements, such as payment channels and WireGuard integration.

Using the Skynet-style payment solution where providers are paid for each packet they send, and clients pay as they receive data, can create a more efficient CDN. This system can integrate with existing retrieval systems (e.g., the Filecoin storage incentives) to encourage more accessible and faster content retrieval.

However, this solution raises several questions, such as how to best integrate payments into the transport layer and how these incentives can be mixed with existing systems. There are various possibilities, such as using WireGuard or TCP/IP, strapping hash-based Monopoly money to state channels, and more.

To move forward with these ideas, proposals include:

1. Modeling the tit-for-tat incentive layer and conducting more in-depth research on these potential solutions.
2. Implementing a test version of a state channel with a transport protocol like WireGuard.

There are also questions regarding the most straightforward way to incorporate bandwidth payments with FVM, IPVM, or other relevant protocols.

Ultimately, integrating these improvements and solutions with existing systems like Filecoin storage incentives, Saturn, and others, can help pave the way to a more efficient and decentralized CDN.

## Key Takeaways

- Current solutions like Skynet and retrieval pinning have limitations when it comes to CDN speed and scalability.
- A solution combining Skynet-style incentives and WireGuard integration can potentially create a more efficient CDN.
- Integrating the proposed improvements with existing systems like Filecoin storage incentives and Saturn can improve decentralized data retrieval.
- More research and modeling work is needed to understand the best way to integrate these ideas into existing systems and to ensure their efficiency and scalability.