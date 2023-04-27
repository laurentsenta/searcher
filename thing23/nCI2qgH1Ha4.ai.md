# Compute on Data in Space with CryptoSat

<https://youtube.com/watch?v=nCI2qgH1Ha4>

![image for Compute on data in space - Yan Michalevsky](/thing23/nCI2qgH1Ha4.jpg?raw=true)

## Overview

In this video, Yan Michalevsky, one of the founders of Cryptosat, discusses the company's work on compute on data in space, and the integration with Protocol Labs specific projects. He talks about the company's vision of providing a trust infrastructure for Web3, and how they are achieving this by launching, building, integrating, and launching satellites into low Earth orbits for Web3 use cases, cryptographic protocols, and confidential computing. Yan also discusses the use of trusted execution environments for decentralized clouds and explains the concept of attestation.

## Article

Yan Michalevsky discusses the work of Cryptosat and how it is contributing to building a trust infrastructure for Web3. The company achieves this by launching, building, integrating, and launching satellites into low Earth orbits for Web3 use cases, cryptographic protocols, and confidential computing.

One of the main objectives for Cryptosat, as mentioned by Yan, is to provide a trusted execution environment that is physically isolated, secure, and ensures confidentiality and integrity for various sensitive workloads, often processing sensitive data. To achieve this goal, Yan explains, the company uses trusted execution environments, and the lack of any physical access and the ability to compromise anything in memory, use any side channels, and so on. Cryptosat provides this kind of trusted execution environment by launching satellites into low Earth orbits and connecting to a ground station infrastructure with convenient APIs for users to issue requests directly to those satellites.

Yan also talks about the company's vision of getting to a simple, trustful API, plus direct integrations with smart contracts that would enable users to request certain operations and have them completed in space. This has already been achieved to some extent, where Cryptosat provides a simulator known as the CryptoSat simulator, which is an interactive tutorial, satellite tracker, and a playground in JavaScript where developers can try different APIs.

One of the use cases Yan mentions for Cryptosat is providing a trusted execution environment for cryptographic schemes that require public parameters, just some numbers that need to be produced in a certain way, which if not being done correctly, can potentially compromise the entire protocol. Cryptosat provides a trusted execution environment that can do those things for you, and two examples of that were the participation in the KZG ceremony and producing a trusted setup for a ZK-SNARK scheme that's used by the DoraHax DAO that serves for community project funding.

Yan also explains the concept of attestation in this space of trusted execution environments, which is the concept of proving that you're running whatever you want to run in an actual trusted execution environment and not somewhere else. To achieve this, Cryptosat has this key generation ceremony that starts after the launch of each of their satellites, where they generate a keeper or a couple of keepers after the launch. Yan explains that the private keys never leave the satellite, and they start broadcasting the public key for a certain time such that multiple participants, multiple ground stations can receive it, and gossip between them and see that they agree that they're getting the same public key.

Yan concludes the talk by discussing Cryptosat's interest in working with decentralized clouds like Bacayau's architecture and Super Protocol's architecture, among others. In both of these examples, Yan proposes the idea of having a satellite provide a trusted execution environment that can process data and provide results with the attestation that it all happened on an authentic crypto-satellite.

In summary, Cryptosat's work on compute on data in space and trusted execution environments for decentralized clouds is a revolutionary concept in the space of Web3 security and confidentiality. The company's vision of providing a trusted infrastructure for Web3 is indeed a game-changer that will take blockchain technology to the next level. 

## Conclusion

In this talk, Yan Michalevsky discusses the work of Cryptosat and their contribution to building a trust infrastructure for Web3. The company uses trusted execution environments that ensure confidentiality and integrity for various sensitive workloads to achieve this goal. Yan also discusses the company's vision for providing direct integrations with smart contracts and simplifying the API for users. Cryptosat's work is a game-changer in the world of Web3, and the use of trusted execution environments for decentralized clouds will take blockchain technology to the next level.